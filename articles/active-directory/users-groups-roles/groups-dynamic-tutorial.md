---
title: 'Pertenencia dinámica a grupos: tutorial para agregar o quitar usuarios en Azure Active Directory'
description: En este tutorial, se usan grupos con reglas de pertenencia del usuario para agregar o quitar usuarios automáticamente
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: tutorial
ms.date: 01/28/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro;seo-update-azuread-jan
ms.openlocfilehash: bf625b45fa47d4fa681521eb992bd2b07cc08946
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55296254"
---
# <a name="tutorial-add-or-remove-group-members-automatically"></a>Tutorial: Adición o eliminación automáticas de miembros de grupos

En Azure Active Directory (Azure AD), puede agregar o quitar usuarios de grupos de seguridad o grupos de Office 365 automáticamente, por lo que no siempre tiene que hacerlo manualmente. Cada vez que cambian las propiedades de un usuario o un dispositivo, Azure AD evalúa todas las reglas de grupos dinámicos del inquilino para ver si el cambio hace que se deban agregar o quitar miembros.

En este tutorial, aprenderá a:
> [!div class="checklist"]
> * Crear un grupo que se rellena automáticamente de usuarios invitados de una empresa asociada específica.
> * Asignar licencias al grupo para que los usuarios invitados puedan acceder a las características específicas del asociado.
> * Extra: proteger el grupo **Todos los usuarios** mediante la eliminación de los usuarios invitados para que, por ejemplo, pueda otorgar acceso a sitios internos a los usuarios miembros.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Esta característica requiere una licencia Premium de Azure AD como administrador global del inquilino. Si no dispone de una, en Azure AD, seleccione **Licencias** > **Productos** > **Probar/Comprar**.

No es necesario asignar licencias a los usuarios para que sean miembros de grupos dinámicos. Solo necesita el número mínimo de licencias Premium P1 de Azure AD disponibles en el inquilino para dar cobertura a dichos usuarios. 

## <a name="create-a-group-of-guest-users"></a>Creación de un grupo de usuarios invitados

En primer lugar, creará un grupo para los usuarios invitados que proceden de una única empresa asociada. Necesitan una licencia especial, por lo que a menudo resulta más eficaz crear un grupo para este propósito.

1. Inicie sesión en Azure Portal (https://portal.azure.com) con una cuenta que sea el administrador global del inquilino.
2. Seleccione **Azure Active Directory** > **Grupos** > **Nuevo grupo**.
  ![Comando de selección de nuevo grupo](./media/groups-dynamic-tutorial/new-group.png)
3. En la hoja **Grupo**:
  
  * Seleccione **Seguridad** como el tipo de grupo.
  * Escriba `Guest users Contoso` como el nombre y la descripción del grupo.
  * Cambie el **Tipo de pertenencia** a **Usuario dinámico**.
  * Seleccione **Agregar una consulta dinámica**.
  
4. Seleccione **Regla avanzada** y, en el cuadro **Regla avanzada**, escriba: `(user.userType -eq "Guest") -and (user.companyName -eq "Contoso")`
5. Seleccione **Agregar consulta** para cerrar la hoja.
6. En la hoja **Grupo**, seleccione **Crear** para crear el grupo.

## <a name="assign-licenses"></a>Asignación de licencias

Ahora que tiene el nuevo grupo, puede aplicar las licencias que necesitan estos usuarios asociados.

1. En Azure AD, seleccione **Licencias**, seleccione una o varias licencias y, a continuación, seleccione **Asignar**.
2. Seleccione **Usuarios y grupos**, seleccione el grupo **Guest users Contoso** y guarde los cambios.
3. En **Opciones de asignación**, puede activar o desactivar los planes de servicio, incluidas las licencias que ha seleccionado. Cuando se realiza un cambio, asegúrese de hacer clic en **Aceptar** para guardar los cambios.
4. Para completar la asignación, en el panel **Asignar licencia**, haga clic en **Asignar** en la parte inferior del panel.

## <a name="remove-guests-from-all-users-group"></a>Eliminación de los invitados del grupo Todos los usuarios

Quizás su plan administrativo final es asignar todos los usuarios invitados a sus propios grupos por empresa. Ahora también puede cambiar el grupo **Todos los usuarios** de modo que esté reservado solo para los usuarios miembros del inquilino. A continuación, puede usarlo para asignar aplicaciones y licencias que son específicas de su organización principal.

   ![Cambio del grupo Todos los usuarios a solo miembros](./media/groups-dynamic-tutorial/all-users-edit.png)

## <a name="clean-up-resources"></a>Limpieza de recursos

**Para eliminar el grupo de usuarios invitados**

1. Inicie sesión en [Azure Portal](https://portal.azure.com) con una cuenta que sea el administrador global del inquilino.
2. Seleccione **Azure Active Directory** > **Grupos**. Seleccione el grupo **Guest users Contoso**, seleccione los puntos suspensivos (...) y, a continuación, seleccione **Eliminar**. Cuando se elimina el grupo, se eliminan las licencias asignadas.

**Para restaurar el grupo Todos los usuarios**
1. Seleccione **Azure Active Directory** > **Grupos**. Seleccione el nombre del grupo **Todos los usuarios** para abrir el grupo.
1. Seleccione **Reglas de pertenencia dinámica**, borre todo el texto de la regla y seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:
> [!div class="checklist"]
> * Creación de un grupo de usuarios invitados
> * Asignación de licencias al nuevo grupo
> * Cambio del grupo Todos los usuarios a solo miembros

Avance al siguiente artículo para conocer más aspectos básicos de las licencias basadas en grupos
> [!div class="nextstepaction"]
> [Conceptos básicos de las licencias basadas en grupos](../fundamentals/active-directory-licensing-whatis-azure-portal.md)



