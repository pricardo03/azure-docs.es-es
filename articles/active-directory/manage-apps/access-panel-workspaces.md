---
title: Creación de áreas de trabajo para los portales de Mis aplicaciones en Azure Active Directory | Microsoft Docs
description: Use las áreas de trabajo de Mis aplicaciones para personalizar las páginas de Mis aplicaciones con el fin de que los usuarios finales de Mis aplicaciones reciban una experiencia más sencilla. Organice las aplicaciones en grupos con pestañas independientes.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2019
ms.author: mimart
ms.reviewer: kasimpso
ms.collection: M365-identity-device-management
ms.openlocfilehash: e8e1fd51e0190e0f8889112b17b58680ed9329e3
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75443445"
---
# <a name="create-workspaces-on-the-my-apps-preview-portal"></a>Creación de áreas de trabajo en el portal de Mis aplicaciones (versión preliminar)

Los usuarios pueden usar el portal Mis aplicaciones (versión preliminar) para ver e iniciar las aplicaciones basadas en la nube a las que tienen acceso. De forma predeterminada, todas las aplicaciones a las que puede tener acceso un usuario se muestran juntas en una sola página. Para organizar mejor esta página para los usuarios, si tiene una licencia de Azure AD Premium P1 o P2, puede configurar áreas de trabajo. Con un área de trabajo, puede agrupar las aplicaciones que están relacionadas (por ejemplo, por rol de trabajo, tarea o proyecto) y mostrarlas en una pestaña independiente. Un área de trabajo aplica esencialmente un filtro a las aplicaciones a las que un usuario ya puede tener acceso, por lo que el usuario solo ve las aplicaciones del área de trabajo que se le han asignado.

> [!NOTE]
> En este artículo se explica cómo un administrador puede habilitar y crear áreas de trabajo. Para obtener información para el usuario final sobre cómo usar las áreas de trabajo y el portal de Mis aplicaciones, consulte [Acceso y uso de áreas de trabajo](https://docs.microsoft.com/azure/active-directory/user-help/my-applications-portal-workspaces).

## <a name="enable-my-apps-preview-features"></a>Habilitación de las características en versión preliminar de Mis aplicaciones

1. Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como administrador global o coadministrador.

2. Vaya a **Azure Active Directory** > **Configuración de usuario**.

3. En **Versiones preliminares de características de usuarios**, seleccione **Administrar la configuración de la versión preliminar de características del usuario**.

4. En **Los usuarios pueden utilizar las características en versión preliminar de Mis aplicaciones**, elija una de las siguientes opciones:
   * **Seleccionado**: habilita las características en versión preliminar para un grupo específico. Use la opción **Seleccionar un grupo** para seleccionar el grupo para el que desea habilitar las características en versión preliminar.  
   * **Todo**: habilita las características en versión preliminar para todos los usuarios.

   ![Otras características en versión preliminar](media/access-panel-workspaces/user-preview-features.png)

> [!NOTE]
> Para abrir el portal de Mis aplicaciones, los usuarios pueden usar el vínculo `https://myapps.microsoft.com` o el vínculo personalizado de la organización, como `https://myapps.microsoft.com/contoso.com`. Después de habilitar la nueva experiencia de Mis aplicaciones, se mostrará un banner similar a **Una experiencia actualizada de Mis aplicaciones está disponible** en la parte superior de la página Mis aplicaciones y los usuarios podrán seleccionar **Probarlo** para ver la nueva experiencia. Para dejar de usar la nueva experiencia, los usuarios pueden seleccionar **Sí** en el banner similar a **Salir de la nueva experiencia** de la parte superior de la página.

## <a name="create-a-workspace"></a>Crear un área de trabajo

Para crear un área de trabajo, debe tener una licencia de Azure AD Premium P1 o P2.

1. Abra [**Azure Portal**](https://portal.azure.com/) e inicie sesión como administrador con una licencia de Azure AD Premium P1 o P2.

2. Vaya a **Azure Active Directory** > **Aplicaciones empresariales**.

3. En **Administrar**, seleccione **Áreas de trabajo (versión preliminar)** .

4. Seleccione **Nuevo área de trabajo**. En la página **Nueva área de trabajo**, escriba un **nombre** para el área de trabajo (se recomienda no usar "workspace"). Escriba una **descripción**.

   ![Crear un área de trabajo](media/access-panel-workspaces/new-workspace.png)

5. Seleccione **Revisar + crear**. Aparecen las propiedades de la nueva área de trabajo.

6. Seleccione la pestaña **Aplicaciones** . En **Agregar aplicaciones**, seleccione todas las aplicaciones que desee agregar al área de trabajo o use el cuadro **Buscar** para buscar aplicaciones. 

   ![Adición de aplicaciones al área de trabajo](media/access-panel-workspaces/add-applications.png)

7. Seleccione **Agregar**. Aparece la lista de aplicaciones seleccionadas. Puede usar las flechas arriba y abajo para cambiar el orden de las aplicaciones en la lista.

   ![Lista de aplicaciones en el área de trabajo](media/access-panel-workspaces/add-applications-list.png)

8. Seleccione la pestaña **Usuarios y grupos**. Seleccione **Agregar usuario** para agregar un usuario o un grupo. 

9. En la página **Seleccionar miembros**, seleccione los usuarios o grupos a los que desea asignar el área de trabajo. También puede usar el cuadro **Buscar** para buscar usuarios o grupos.

   ![Asignación de usuarios y grupos al área de trabajo](media/access-panel-workspaces/add-users-and-groups.png)

10. Cuando haya terminado de seleccionar usuarios y grupos, elija **Seleccionar**.

11. Para cambiar el rol de un usuario de **Acceso de lectura** a **Propietario** o viceversa, haga clic en el rol actual y seleccione un nuevo rol.

    ![Asignación de roles a usuarios y grupos](media/access-panel-workspaces/users-groups-list-role.png)

## <a name="view-audit-logs"></a>Visualización de registros de auditoría

Los registros de auditoría registran las operaciones de las áreas de trabajo de Mis aplicaciones, incluidas las acciones de usuario final de creación de áreas de trabajo. Los siguientes eventos se generan desde Mis aplicaciones:

* Creación del espacio de trabajo
* Edición del área de trabajo
* Eliminación del área de trabajo
* Inicio de una aplicación (usuario final)
* Adición de aplicaciones de autoservicio (usuario final)
* Eliminación de aplicaciones de autoservicio (usuario final)

Puede obtener acceso a los registros de auditoría de [Azure Portal](https://portal.azure.com) seleccionando **Azure Active Directory** > **Aplicaciones empresariales** > **Registros de auditoría** en la sección Actividad. En **Servicio**, seleccione **Mis aplicaciones**.

   ![Asignación de roles a usuarios y grupos](media/access-panel-workspaces/audit-log-myapps.png)

## <a name="get-support-for-my-account-pages"></a>Obtener soporte técnico para las páginas de Mi cuenta

En la página Mis aplicaciones, un usuario puede seleccionar **Mi cuenta** > **Ver mi cuenta** para abrir la configuración de su cuenta. En la página de Azure AD **Mi cuenta**, los usuarios pueden administrar la información de seguridad, los dispositivos, las contraseñas, etc. También pueden tener acceso a la configuración de su cuenta de Office.

En caso de que necesite enviar una solicitud de soporte técnico por un problema con la página de la cuenta de Azure AD o la cuenta de Office, siga estos pasos para que la solicitud se gestione correctamente: 

* En caso de tener problemas con la página **"Mi cuenta" de Azure AD**, abra una solicitud de soporte técnico desde Azure Portal. Vaya a **Azure Portal** > **Azure Active Directory** > **Nueva solicitud de soporte técnico**.

* En caso de tener problemas con la página **"Mi cuenta" de Office**, abra una solicitud de soporte técnico desde el centro de administración de Microsoft 365. Vaya a **Centro de administración de Microsoft 365** > **Soporte técnico**. 

## <a name="next-steps"></a>Pasos siguientes
[Experiencias de usuario final para aplicaciones en Azure Active Directory](end-user-experiences.md)