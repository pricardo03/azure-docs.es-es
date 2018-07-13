---
title: Creación de un grupo para usuarios de Azure AD | Microsoft Docs
description: Procedimiento para crear un grupo en Azure Active Directory y agregar miembros al grupo
services: active-directory
documentationcenter: ''
author: eross-msft
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: fundamentals
ms.topic: quickstart
ms.date: 08/04/2017
ms.author: lizross
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 3c71c9c49413045e3a730c10e90ea3c12648b4cb
ms.sourcegitcommit: 0b4da003fc0063c6232f795d6b67fa8101695b61
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37857732"
---
# <a name="create-a-group-and-add-members-in-azure-active-directory"></a>Creación de un grupo y adición de miembros en Azure Active Directory
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-groups-create-azure-portal.md)
> * [PowerShell](../users-groups-roles/groups-settings-v2-cmdlets.md)

En este artículo se explica cómo crear y rellenar un nuevo grupo en Azure Active Directory. Use un grupo para realizar tareas de administración, como asignar licencias o permisos a un número de usuarios o dispositivos a la vez.

## <a name="how-do-i-create-a-group"></a>¿Cómo se crea un grupo?
1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta que tenga el rol de administrador global en el directorio.
2. Seleccione **Todos los servicios**, escriba **Usuarios y grupos** en el cuadro de texto y, después, presione **Entrar**.

   ![Apertura de Administración de usuarios](./media/active-directory-groups-create-azure-portal/search-user-management.png)
3. En la hoja **Usuarios y grupos**, seleccione **Todos los grupos**.

   ![Apertura de la hoja Grupos](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)
4. En la hoja **Usuarios y grupos - Todos los grupos**, seleccione el comando **Agregar**.

   ![Selección del comando Agregar](./media/active-directory-groups-create-azure-portal/add-group-command.png)
5. En la hoja **Grupo** , agregue un nombre y una descripción para el nuevo grupo.
6. Para seleccionar los miembros que se agregarán al grupo, seleccione **Asignado** en el cuadro **Membership type** (Tipo de pertenencia a grupos) y luego seleccione **Miembros**. Para obtener más información sobre cómo administrar la pertenencia de un grupo de forma dinámica, consulte el artículo sobre el [uso de atributos para crear reglas avanzadas de pertenencias a grupo](../users-groups-roles/groups-dynamic-membership.md).

   ![Selección de miembros para agregar](./media/active-directory-groups-create-azure-portal/select-members.png)
7. En la hoja **Miembros**, seleccione uno o varios usuarios o dispositivos que quiera agregar al grupo y, luego, haga clic en el botón **Seleccionar** situado en la parte inferior de la hoja para agregarlos. El cuadro **Usuario** filtra la visualización en función de si coincide lo que ha escrito con cualquier parte del nombre de un usuario o dispositivo. No se aceptan caracteres comodín en el cuadro.
8. Cuando termine de agregar miembros al grupo, seleccione **Crear** en la hoja **Grupo**.    

   ![Creación de la confirmación de grupo](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)


## <a name="next-steps"></a>Pasos siguientes
Estos artículos proporcionan información adicional sobre Azure Active Directory.

* [Consulta de los grupos existentes](active-directory-groups-view-azure-portal.md)
* [Administración de la configuración de un grupo](active-directory-groups-settings-azure-portal.md)
* [Administrar miembros de un grupo](active-directory-groups-members-azure-portal.md)
* [Administrar la pertenencia a grupos](active-directory-groups-membership-azure-portal.md)
* [Administrar reglas dinámicas de los usuarios de un grupo](../users-groups-roles/groups-dynamic-membership.md)
