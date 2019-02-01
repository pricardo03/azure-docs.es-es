---
title: Adición o actualización de la información de perfil de un usuario en Azure Active Directory | Microsoft Docs
description: Instrucciones sobre cómo agregar información a un perfil de usuario en Azure Active Directory, incluidos una imagen y los detalles del trabajo.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 09/05/2018
ms.author: lizross
ms.reviewer: jeffsta
ms.openlocfilehash: d526a56f0d33ebaba664a455b9e541f0f2489a53
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "55104531"
---
# <a name="add-or-update-a-users-profile-information-using-azure-active-directory"></a>Adición o actualización de la información de perfil de un usuario mediante Azure Active Directory
Agregue información de perfil de usuario, como la imagen de perfil, información específica del trabajo y algunos valores de configuración mediante Azure Active Directory (Azure AD). Para obtener más información acerca de la adición de nuevos usuarios, consulte [cómo agregar o eliminar usuarios en Azure Active Directory](add-users-azure-active-directory.md).

## <a name="add-or-change-profile-information"></a>Adiciones o cambios a la información del perfil
Como verá, hay más información disponible en un perfil de usuario que la que puede agregarse durante la creación del usuario. Toda esta información adicional es opcional y se puede agregar según sea necesario para su organización.

## <a name="to-add-or-change-profile-information"></a>Para agregar o cambiar la información del perfil
1. Inicie sesión en [Azure Portal](https://portal.azure.com/) como administrador global o administrador de usuarios del directorio.

2. Seleccione **Azure Active Directory**, **Usuarios** y, a continuación, seleccione un usuario. Por ejemplo, _Alain Charon_.

    Se muestra la página **Alain Charon - Perfil**.

    ![Página de perfil del usuario, incluida la información editable](media/active-directory-users-profile-azure-portal/user-profile-all-blade.png)

3. Seleccione **Editar** para agregar o actualizar de forma opcional la información incluida en cada una de las secciones disponibles.

    ![Página de perfil del usuario, se muestran las áreas editables](media/active-directory-users-profile-azure-portal/user-profile-edit.png)

    - **Imagen de perfil.** Seleccione una imagen en miniatura para la cuenta de usuario. Esta imagen aparece en Azure Active Directory y en las páginas personales del usuario, tales como la página myapps.microsoft.com.

    - **Identidad.** Agregue cualquier información relacionada con la cuenta, como un apellido de casado o un nombre de usuario modificado. 

    - **Información del trabajo.** Agregue cualquier información relacionada con el trabajo, como el puesto, departamento o administrador del usuario.

    - **Configuración.** Establezca si el usuario puede iniciar sesión en el inquilino de Azure Active Directory. También puede especificar la ubicación global del usuario.

    - **Información de contacto.** Agregue cualquier información de contacto pertinente para el usuario. Por ejemplo, una dirección o un número de celular.

    - **Información de contacto para la autenticación.** Compruebe esta información para asegurarse de que hay una dirección de correo electrónico y un número de teléfono activos para el usuario. Azure Active Directory usa esta información para comprobar la identidad del usuario durante el inicio de sesión. Solo un administrador global puede actualizar la información de contacto para la autenticación.

4. Seleccione **Guardar**.

    Todos los cambios se guardan para el usuario.

    >[!Note]
    >Debe usar Windows Server Active Directory para actualizar la identidad, la información de contacto o la información del trabajo para los usuarios cuya fuente de autoridad es Windows Server Active Directory. Después de completar la actualización, debe esperar a que se complete el próximo ciclo de sincronización antes de poder ver los cambios.

## <a name="next-steps"></a>Pasos siguientes
Después de actualizar los perfiles de los usuarios, puede realizar los siguientes procesos básicos:

- [Adición o eliminación de usuarios](add-users-azure-active-directory.md)

- [Asignación de roles a usuarios](active-directory-users-assign-role-azure-portal.md)

- [Creación de un grupo básico e incorporación de miembros](active-directory-groups-create-azure-portal.md)

O bien, puede realizar otras tareas de administración de usuarios, como asignar delegados, usar directivas y compartir cuentas de usuario. Para obtener más información acerca de otras acciones disponibles, consulte la [documentación de administración de usuarios en Azure Active Directory](../users-groups-roles/index.yml).
