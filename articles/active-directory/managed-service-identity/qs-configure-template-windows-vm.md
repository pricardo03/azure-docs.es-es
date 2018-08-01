---
title: Configuración de Managed Service Identity en una máquina virtual de Azure mediante una plantilla
description: Instrucciones paso a paso para configurar una identidad de Managed Service Identity en una máquina virtual de Azure mediante una plantilla de Azure Resource Manager.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/14/2017
ms.author: daveba
ms.openlocfilehash: 15a743f524c58e56247ec46fee27611b33595bad
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2018
ms.locfileid: "39258701"
---
# <a name="configure-a-vm-managed-service-identity-by-using-a-template"></a>Configuración de Managed Service Identity (MSI) en una máquina virtual mediante una plantilla

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Managed Service Identity proporciona a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory. Puede usar esta identidad para autenticar a cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. 

En este artículo, aprenderá a realizar las siguientes operaciones de Managed Service Identity en una máquina virtual de Azure mediante la plantilla de implementación de Azure Resource Manager:

## <a name="prerequisites"></a>Requisitos previos

- Si no está familiarizado con Managed Service Identity, consulte la [sección de introducción](overview.md). **No olvide revisar la [diferencia entre una identidad asignada por el sistema y una asignada por el usuario](overview.md#how-does-it-work)**.
- Si aún no tiene una cuenta de Azure, [regístrese para una cuenta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para llevar a cabo las operaciones de administración de este artículo, su cuenta debe tener las siguientes asignaciones de roles:
    - [Colaborador de máquina virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) para crear una máquina virtual y habilitar y quitar la identidad administrada asignada por el usuario o el sistema desde una máquina virtual de Azure.
    - Rol [Colaborador de identidad administrada](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) para crear una identidad asignada por el usuario.
    - Rol [Operador de identidad administrada](/azure/role-based-access-control/built-in-roles#managed-identity-operator) para asignar y quitar una identidad asignada por el usuario en una máquina virtual.

## <a name="azure-resource-manager-templates"></a>Plantillas del Administrador de recursos de Azure

Al igual que con Azure Portal y los scripts, las plantillas de [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) proporcionan la capacidad de implementar recursos nuevos o modificados definidos por un grupo de recursos de Azure. Existen varias opciones para la edición e implementación de plantillas, tanto localmente como basadas en el portal, incluidas:

   - Usar una [plantilla personalizada de Azure Marketplace](../../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), que permite crear una plantilla desde cero o basada en una común existente o [plantilla de inicio rápido](https://azure.microsoft.com/documentation/templates/).
   - Derivar a partir de un grupo de recursos existente, exportando una plantilla de [la implementación original](../../azure-resource-manager/resource-manager-export-template.md#view-template-from-deployment-history) o del [estado actual de la implementación](../../azure-resource-manager/resource-manager-export-template.md#export-the-template-from-resource-group).
   - Usar un [editor de JSON (por ejemplo, VS Code)](../../azure-resource-manager/resource-manager-create-first-template.md) local y, a continuación, cargarla e implementarla con PowerShell o la CLI.
   - Usar el [proyecto del grupo de recursos de Azure](../../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) de Visual Studio tanto para crear como para implementar una plantilla.  

Independientemente de la opción que elija, la sintaxis de la plantilla es la misma durante la implementación inicial y posteriores implementaciones. La habilitación de una identidad asignada por el sistema o el usuario en una VM existente o nueva se realiza de la misma forma. Asimismo, de forma predeterminada, Azure Resource Manager realiza una [actualización incremental](../../azure-resource-manager/resource-group-template-deploy.md#incremental-and-complete-deployments) en las implementaciones.

## <a name="system-assigned-identity"></a>Identidad asignada por el sistema

En esta sección, se habilita y deshabilita una identidad asignada por el sistema con una plantilla de Azure Resource Manager.

### <a name="enable-system-assigned-identity-during-creation-of-an-azure-vm-or-on-an-existing-vm"></a>Habilitación de una identidad asignada por el sistema durante la creación de una máquina virtual o en una máquina virtual existente de Azure

1. Independientemente de que inicie sesión localmente en Azure o mediante Azure Portal, use una cuenta que esté asociada a la suscripción de Azure que contiene la máquina virtual.

2. Después de cargar la plantilla en un editor, busque el recurso de interés `Microsoft.Compute/virtualMachines` dentro de la sección `resources`. El suyo puede diferir ligeramente con respecto a la siguiente captura de pantalla, en función del editor que use y de si está editando una plantilla para una implementación nueva o una existente.

   >[!NOTE] 
   > En este ejemplo, se asumen que se han definido variables como `vmName`, `storageAccountName` y `nicName` en la plantilla.
   >

   ![Captura de pantalla de la plantilla - localizar máquina virtual](../managed-service-identity/media/msi-qs-configure-template-windows-vm/template-file-before.png) 

3. Para habilitar la identidad asignada por el sistema, agregue la propiedad `"identity"` en el mismo nivel que la propiedad `"type": "Microsoft.Compute/virtualMachines"`. Use la sintaxis siguiente:

   ```JSON
   "identity": { 
       "type": "systemAssigned"
   },
   ```

4. (Opcional) Agregue la extensión de Managed Service Identity de máquina virtual como un elemento `resources`. Este paso es opcional, ya que puede usar el punto de conexión de identidad de Azure Instance Metadata Service (IMDS) para recuperar tokens.  Use la sintaxis siguiente:

   >[!NOTE] 
   > En el ejemplo siguiente se da por hecho que se está implementando una extensión (`ManagedIdentityExtensionForWindows`) de máquina virtual Windows. También puede configurarlo para Linux utilizando `ManagedIdentityExtensionForLinux` en su lugar, para los elementos `"name"` y `"type"`.
   >

   ```JSON
   { 
       "type": "Microsoft.Compute/virtualMachines/extensions",
       "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
       "apiVersion": "2016-03-30",
       "location": "[resourceGroup().location]",
       "dependsOn": [
           "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
       ],
       "properties": {
           "publisher": "Microsoft.ManagedIdentity",
           "type": "ManagedIdentityExtensionForWindows",
           "typeHandlerVersion": "1.0",
           "autoUpgradeMinorVersion": true,
           "settings": {
               "port": 50342
           },
           "protectedSettings": {}
       }
   }
   ```

5. Cuando haya terminado, la plantilla debería tener una apariencia similar a la siguiente:

   ![Captura de pantalla de la plantilla después de la actualización](../managed-service-identity/media/msi-qs-configure-template-windows-vm/template-file-after.png)

### <a name="assign-a-role-the-vms-system-assigned-identity"></a>Asignación de un rol a la identidad asignada por el sistema de la máquina virtual

Después de haber habilitado la identidad asignada por el sistema en la máquina virtual, es aconsejable concederle un rol con el acceso de **lector** al grupo de recursos en el que se creó.

1. Independientemente de que inicie sesión localmente en Azure o mediante Azure Portal, use una cuenta que esté asociada a la suscripción de Azure que contiene la máquina virtual.
 
2. Cargue la plantilla en un [editor](#azure-resource-manager-templates) y agregue la siguiente información para dar a la máquina virtual acceso de **lector** al grupo de recursos en el que se creó.  La estructura de la plantilla puede variar según el editor y el modelo de implementación que elija.
   
   Agregue lo siguiente bajo la sección `parameters`:

    ```JSON
    "builtInRoleType": {
          "type": "string",
          "defaultValue": "Reader"
        },
        "rbacGuid": {
          "type": "string"
        }
    ```

    Agregue lo siguiente bajo la sección `variables`:

    ```JSON
    "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]"
    ```

    Agregue lo siguiente bajo la sección `resources`:

    ```JSON
    {
        "apiVersion": "2017-09-01",
         "type": "Microsoft.Authorization/roleAssignments",
         "name": "[parameters('rbacGuid')]",
         "properties": {
                "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
                "principalId": "[reference(variables('vmResourceId'), '2017-12-01', 'Full').identity.principalId]",
                "scope": "[resourceGroup().id]"
          },
          "dependsOn": [
                "[concat('Microsoft.Compute/virtualMachines/', parameters('vmName'))]"
            ]
    }
    ```

### <a name="disable-a-system-assigned-identity-from-an-azure-vm"></a>Deshabilitación de una identidad asignada por el sistema de una máquina virtual de Azure

Si tiene una máquina virtual que ya no necesita una identidad de servicio administrada:

1. Independientemente de que inicie sesión localmente en Azure o mediante Azure Portal, use una cuenta que esté asociada a la suscripción de Azure que contiene la máquina virtual.

2. Cargue la plantilla en un [editor](#azure-resource-manager-templates) y busque el `Microsoft.Compute/virtualMachines`recurso de interés dentro de la sección `resources`. Si dispone de una máquina virtual que solo tenga una identidad asignada por el sistema, puede deshabilitarla cambiando el tipo de identidad a `None`.  Si la máquina virtual tiene tanto las identidades asignadas por el usuario como las del sistema, quite `SystemAssigned` del tipo de identidad y mantenga `UserAssigned` junto con la matriz `identityIds` de las identidades asignadas por el usuario.  En el ejemplo siguiente se muestra cómo quitar una identidad asignada por el sistema de una máquina virtual sin identidades asignadas por el usuario:
   
   ```JSON
    {
      "apiVersion": "2017-12-01",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "identity": { 
          "type": "None"
    }
   ```

## <a name="user-assigned-identity"></a>Identidad asignada por el usuario

En esta sección, asignará una identidad asignada por el usuario a una máquina virtual de Azure mediante la plantilla de Azure Resource Manager.

> [!Note]
> Para crear una identidad asignada por el usuario usando una plantilla de Azure Resource Manager, consulte [Create a user assigned identity](how-to-manage-ua-identity-arm.md#create-a-user-assigned-identity) (Creación de una identidad asignada por el usuario).

 ### <a name="assign-a-user-assigned-identity-to-an-azure-vm"></a>Asignación de una identidad asignada por el usuario a una VM de Azure

1. En el elemento `resources`, agregue la siguiente entrada para asignar una identidad asignada por el usuario a la VM.  No olvide reemplazar `<USERASSIGNEDIDENTITY>` con el nombre de la identidad asignada por el usuario que ha creado.
   
   > [!Important]
   > El valor `<USERASSIGNEDIDENTITYNAME>` que se muestra en el ejemplo siguiente se debe almacenar en una variable.  Además, para la implementación actualmente compatible de la asignación de identidades asignadas del usuario a una máquina virtual en una plantilla de Resource Manager, la versión de API debe coincidir con la versión del ejemplo siguiente.
    
    ```json
    {
        "apiVersion": "2017-12-01",
        "type": "Microsoft.Compute/virtualMachines",
        "name": "[variables('vmName')]",
        "location": "[resourceGroup().location]",
        "identity": {
            "type": "userAssigned",
            "identityIds": [
                "[resourceID('Micrososft.ManagedIdentity/userAssignedIdentities/',variables('<USERASSIGNEDIDENTITYNAME>'))]"
            ]
        },
    ```
    
2. (Opcional) A continuación, en el elemento `resources`, agregue la siguiente entrada para asignar la extensión de identidad administrada a la máquina virtual. Este paso es opcional, ya que puede usar el punto de conexión de identidad de Azure Instance Metadata Service (IMDS) para recuperar tokens. Use la sintaxis siguiente:
    ```json
    {
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "[concat(variables('vmName'),'/ManagedIdentityExtensionForWindows')]",
        "apiVersion": "2015-05-01-preview",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
        ],
        "properties": {
            "publisher": "Microsoft.ManagedIdentity",
            "type": "ManagedIdentityExtensionForWindows",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "port": 50342
            }
        }
    }
    ```
    
3.  Cuando haya terminado, la plantilla debería tener un aspecto similar al siguiente:

      ![Captura de pantalla de la identidad asignada por el usuario](./media/qs-configure-template-windows-vm/qs-configure-template-windows-vm-ua-final.PNG)

### <a name="remove-user-assigned-identity-from-an-azure-vm"></a>Eliminación de una identidad asignada por el usuario de una máquina virtual de Azure

Si tiene una máquina virtual que ya no necesita una identidad de servicio administrada:

1. Independientemente de que inicie sesión localmente en Azure o mediante Azure Portal, use una cuenta que esté asociada a la suscripción de Azure que contiene la máquina virtual.

2. Cargue la plantilla en un [editor](#azure-resource-manager-templates) y busque el `Microsoft.Compute/virtualMachines`recurso de interés dentro de la sección `resources`. Si dispone de una máquina virtual que solo tenga una identidad asignada por el usuario, puede deshabilitarla cambiando el tipo de identidad a `None`.  Si la máquina virtual tiene identidades asignadas tanto por el usuario como por el sistema y desea conservar la identidad asignada por el sistema, quite `UserAssigned` del tipo de identidad junto con la matriz `identityIds` de las identidades asignadas por el usuario.
    
   Para quitar una identidad asignada por un usuario único desde una máquina virtual, quítela de la matriz `identityIds`.
   
   En el ejemplo siguiente se muestra cómo quitar todas las identidades asignada por un usuario de una máquina virtual sin identidades asignadas por el sistema:
   
   ```JSON
    {
      "apiVersion": "2017-12-01",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "identity": { 
          "type": "None"
    }
   ```

## <a name="related-content"></a>Contenido relacionado

- Para obtener una perspectiva más amplia acerca de Managed Service Identity, consulte la [Introducción a Managed Service Identity](overview.md).

