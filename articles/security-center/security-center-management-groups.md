---
title: Obtención de visibilidad de todos los inquilinos en Azure Security Center | Microsoft Docs
description: Información sobre cómo obtener visibilidad en todo el inquilino en Azure Security Center.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: b85c0e93-9982-48ad-b23f-53b367f22b10
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/22/2018
ms.author: terrylan
ms.openlocfilehash: 2c95b06ce34b850d1bfaf60e47d6e5fede148a38
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37025561"
---
# <a name="gain-tenant-wide-visibility-for-azure-security-center"></a>Obtención de visibilidad de todos los inquilinos en Azure Security Center
Este artículo le ayuda con la realización de varias acciones que maximizan las ventajas que proporciona Azure Security Center. La realización de estas acciones le permite obtener visibilidad en todas las suscripciones de Azure que están vinculadas al inquilino de Azure Active Directory y administrar de un modo eficaz la posición de seguridad de la organización a escala mediante la aplicación de directivas de seguridad en varias suscripciones de forma agregada.

## <a name="management-groups"></a>Grupos de administración
Los grupos de administración de Azure permiten administrar de un modo eficaz el acceso, las directivas y los informes en grupos de suscripciones, además de administrar todas las inversiones en Azure mediante acciones en el grupo de administración raíz. Cada inquilino de Azure AD tiene un grupo de administración de nivel superior único llamado grupo de administración raíz. Este grupo de administración raíz está integrado en la jerarquía de manera que contiene todos los grupos de administración y suscripciones. Este grupo permite que las directivas globales y las asignaciones de RBAC se apliquen en el nivel de directorio. 

El grupo de administración raíz se crea de manera automática cuando realiza cualquiera de las siguientes acciones: 
1. Decide utilizar los grupos de administración de Azure en **Grupos de administración** en [Azure Portal](https://portal.azure.com).
2. Crea un grupo de administración mediante una llamada API.
3. Crea un grupo de administración con PowerShell.

Para más información sobre los grupos de administración, consulte el artículo [Organización de los recursos con grupos de administración de Azure](../azure-resource-manager/management-groups-overview.md).

## <a name="create-a-management-group-in-the-azure-portal"></a>Creación de un grupo de administración en Azure Portal
Puede organizar las suscripciones en grupos de administración y aplicar las directivas de gobierno a los grupos de administración. Todas las suscripciones dentro de un grupo de administración heredan automáticamente las directivas que se aplican al grupo de administración. Aunque los grupos de administración no son necesarios para incorporar Security Center, se recomienda crear al menos un grupo de administración para que se cree el grupo de administración raíz. Una vez creado el grupo, todas las suscripciones del inquilino de Azure AD estarán vinculadas a él. Para las instrucciones de PowerShell y otra información, consulte [Creación de grupos de administración para la administración de los recursos y la organización](../azure-resource-manager/management-groups-create.md).

 
1. Inicie sesión en el [Azure Portal](http://portal.azure.com).
2. Seleccione **Todos los servicios** > **Grupos de administración**.
3. En la página principal, seleccione **Nuevo grupo de administración**. 

    ![Grupo principal](./media/security-center-management-groups/main.png) 
4.  Rellene el campo de identificador de grupo de administración. 
    - El **identificador de grupo de administración** es el identificador único de directorio que se usa para enviar comandos en este grupo de administración. Este identificador no es editable una vez creado, dado que se usa en todo el sistema de Azure para identificar este grupo. 
    - El campo de nombre para mostrar es el nombre que se muestra en Azure Portal. Un nombre para mostrar independiente es un campo opcional al crear el grupo de administración y se puede cambiar en cualquier momento.  

      ![Crear](./media/security-center-management-groups/create_context_menu.png)  
5.  Seleccione **Guardar**.

### <a name="view-management-groups-in-the-azure-portal"></a>Visualización de grupos de administración en Azure Portal
1. Inicie sesión en [Azure Portal](http://portal.azure.com).
2. Para ver los grupos de administración, seleccione **Todos los servicios** en el menú principal de Azure.
3. En **General**, seleccione **Grupos de administración**.

    ![Creación de un grupo de administración](./media/security-center-management-groups/all-services.png)

## <a name="grant-tenant-level-visibility-and-the-ability-to-assign-policies"></a>Concesión de visibilidad en el nivel de inquilino y la capacidad de asignar directivas

Para obtener visibilidad en la posición de seguridad de todas las suscripciones registradas en el inquilino de Azure AD, se necesita asignar un rol de RBAC con permisos de lectura suficientes en el grupo de administración raíz.

### <a name="elevate-access-for-a-global-administrator-in-azure-active-directory"></a>Elevación de los privilegios de acceso de un administrador global en Azure Active Directory
Un administrador de inquilino de Azure Active Directory no tiene acceso directo a las suscripciones de Azure. No obstante, como administrador de directorio, tienen el derecho de elevarse a sí mismos a un rol que tiene derechos de acceso. Un administrador de inquilino de Azure AD debe elevarse a sí mismo a administrador de acceso de usuarios en el nivel de grupo de administración raíz para poder asignar roles de RBAC. Para obtener instrucciones de PowerShell e información adicional, consulte [Elevación de los privilegios de acceso de un administrador global en Azure Active Directory](../role-based-access-control/elevate-access-global-admin.md). 


1. Inicie sesión en el [Azure Portal](https://portal.azure.com) o en el [Centro de administración de Azure Active Directory](https://aad.portal.azure.com).

2. En la lista de navegación, haga clic en **Azure Active Directory** y, luego, haga clic en **Propiedades**.

   ![Propiedades de Azure AD, captura de pantalla](./media/security-center-management-groups/aad-properties.png)

3. En **El administrador global puede administrar las suscripciones a Azure y los grupos de administración**, seleccione **Sí**.

   ![El administrador global puede gestionar las suscripciones s Azure y los grupos de administración (captura de pantalla)](./media/security-center-management-groups/aad-properties-global-admin-setting.png)

   - Si selecciona **Sí**, se agrega la cuenta de administrador global (usuario conectado actualmente) al rol de administrador de accesos de usuario en RBAC de Azure en el ámbito raíz (`/`), que le concede acceso para ver e informar sobre todas las suscripciones a Azure asociadas al inquilino de Azure AD.

   - Si selecciona **No**, se quita la cuenta de administrador global (usuario conectado actualmente) del rol de administrador de accesos de usuario en RBAC de Azure. No se pueden ver todas las suscripciones a Azure asociadas con el inquilino de Azure AD. Solo se pueden ver y administrar las suscripciones a Azure a las que se le ha concedido acceso.

4. Haga clic en **Guardar** para guardar la configuración.

    - Esta configuración no es una propiedad global y se aplica solo al usuario que tiene la sesión iniciada.

5. Realice las tareas que debe realizar con privilegios de acceso elevados. Cuando haya terminado, establezca el conmutador de nuevo en **No**.

### <a name="assign-rbac-roles-to-users"></a>Asignación de roles de RBAC a los usuarios
Una vez que el administrador de inquilino tiene privilegios de acceso elevados, puede asignar un rol de RBAC a los usuarios correspondientes en el nivel de grupo de administración raíz. Se recomienda asignar el rol de [**Lector**](../role-based-access-control/built-in-roles.md#reader). Este rol es necesario para proporcionar visibilidad en el nivel de inquilino. El rol asignado se propagará automáticamente a todos los grupos de administración y todas las suscripciones del grupo de administración raíz. Para más información sobre los roles de RBAC, consulte [Roles disponibles](../active-directory/active-directory-assign-admin-roles-azure-portal.md#available-roles).

1. Instale [Azure PowerShell](/powershell/azure/install-azurerm-ps).
2. Ejecute los comandos siguientes: 

    ```azurepowershell
    # Install Management Groups Powershell module
    Install-Module AzureRM.Resources
    
    # Login to Azure as a Global Administrator user
    Login-AzureRmAccount
    ```

3. Cuando se le solicite, inicie sesión con credenciales de administrador global. 

    ![Captura de pantalla de la solicitud de inicio de sesión](./media/security-center-management-groups/azurerm-sign-in.PNG)

4. Conceda permisos de rol de lector mediante la ejecución del comando siguiente:

    ```azurepowershell
    # Add Reader role to the required user on the Root Management Group
    # Replace "user@domian.com” with the user to grant access to
    New-AzureRmRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Reader" -Scope "/"
    ```
5. Para eliminar el rol, use el siguiente comando: 

    ```azurepowershell
    Remove-AzureRmRoleAssignment -SignInName "user@domain.com" -RoleDefinitionName "Reader" -Scope "/" 
    ```

<!-- Currently, PowerShell method only 6/26/18

1. Sign in to the [Azure portal](https://portal.azure.com). 
2. To view management groups, select **All services** under the Azure main menu then select **Management Groups**.
3.  Select a management group and click **details**.

    ![Management Groups details screenshot](./media/security-center-management-groups/management-group-details.PNG)
 
4. Click **Access control (IAM)** then **Add**.
5. Select the role to assign and the user, then click **Save**.  
   
   ![Add Security Reader role screenshot](./media/security-center-management-groups/asc-security-reader.png)
-->

### <a name="remove-elevated-access"></a>Eliminación de privilegios de acceso elevados 
Después de asignar los roles de RBAC a los usuarios, el administrador de inquilino debería eliminarse a sí mismo del rol de administrador de acceso de usuarios.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com) o en el [Centro de administración de Azure Active Directory](https://aad.portal.azure.com).

2. En la lista de navegación, haga clic en **Azure Active Directory** y, luego, haga clic en **Propiedades**.

3. En **El administrador global puede administrar suscripciones de Azure y grupos de administración**, seleccione **No**.

4. Haga clic en **Guardar** para guardar la configuración.

### <a name="open-or-refresh-security-center"></a>Apertura o actualización de Security Center
Una vez asignados los roles de RBAC, abra o actualice Azure Security Center para comprobar que tiene visibilidad en todas las suscripciones del inquilino de Azure AD. 

1. Inicie sesión en el [Azure Portal](https://portal.azure.com). 
2. Seleccione **Todos los servicios** en el menú principal de Azure y, a continuación, seleccione **Security Center**.
3. En **Información general** hay un gráfico de cobertura de suscripciones. 
    ![Captura de pantalla del gráfico de cobertura de suscripciones](./media/security-center-management-groups/security-center-subscription-coverage.png)
4. Haga clic en **Cobertura** para ver la lista de suscripciones incluidas. 
    ![Captura de pantalla de la lista de cobertura de suscripciones](./media/security-center-management-groups/security-center-coverage.png)

## <a name="adding-subscriptions-to-a-management-groups"></a>Adición de suscripciones a un grupo de administración
Puede agregar suscripciones al grupo de administración que ha creado. Estos pasos no son obligatorios para obtener visibilidad en todo el inquilino y administración de directivas globales y de acceso.

1. En **Grupos de administración**, seleccione el grupo de administración al que va a agregar la suscripción.

    ![Selección de un grupo de administración para agregar la suscripción](./media/security-center-management-groups/management-group-subscriptions.png)

2. Seleccione **Agregar existente**.

    ![Agregar existente](./media/security-center-management-groups/add-existing.png)

3. Escriba la suscripción en **Agregar recurso existente** y haga clic en **Guardar**.

4. Repita los pasos 1 a 3 hasta que haya agregado todas las suscripciones del ámbito.

 > [!NOTE]
 > Los grupos de administración pueden contener tanto suscripciones como grupos de administración secundarios. Cuando asigna un rol de RBAC a un usuario en el grupo de administración primario, las suscripciones del grupo de administración secundario heredan el acceso. Las directivas establecidas en el grupo de administración primario también son heredadas por los secundarios. 

## <a name="next-steps"></a>Pasos siguientes
En este artículo, ha aprendido a obtener la visibilidad de todos los inquilinos en Azure Security Center. Para más información sobre Security Center, consulte los siguientes artículos:

> [!div class="nextstepaction"]
> [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md)

> [!div class="nextstepaction"]
> [Administración y respuesta a alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md)

