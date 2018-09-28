---
title: 'Habilitación de la autenticación de Azure Active Directory a través de SMB para Azure Files (versión preliminar): Azure Storage'
description: Aprenda a habilitar la autenticación basada en identidades a través de SMB (Bloque de mensajes del servidor) (versión preliminar) para Azure Files mediante Azure Active Directory (Azure AD) Domain Services. Sus máquinas virtuales Windows unidas al dominio pueden entonces acceder a los recursos compartidos de archivos de Azure con las credenciales de Azure AD.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 09/19/2018
ms.author: tamram
ms.openlocfilehash: 31bfcd99e23cc7fe0575a293e3dc22507dbd9e13
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973276"
---
# <a name="enable-azure-active-directory-authentication-over-smb-for-azure-files-preview"></a>Habilitación de la autenticación de Azure Active Directory a través de SMB para Azure Files (versión preliminar)

[!INCLUDE [storage-files-aad-auth-include](../../../includes/storage-files-aad-auth-include.md)]

Para obtener información general de la autenticación de Azure AD a través de SMB de Azure Files, consulte [Introducción a la autenticación de Azure Active Directory a través de SMB para Azure Files (versión preliminar)](storage-files-active-directory-overview.md).

## <a name="workflow-overview"></a>Introducción al flujo de trabajo

Antes de habilitar Azure AD a través de SMB para Azure Files, compruebe que los entornos de Azure Storage y Azure AD se han configurado correctamente. Se recomienda que revise los [requisitos previos](#prerequisites) para asegurarse de que ha realizado todos los pasos necesarios. 

A continuación, conceda acceso a los recursos de Azure Files con las credenciales de Azure AD; para ello, siga estos pasos: 

1. Habilite la autenticación de Azure AD a través de SMB para la cuenta de almacenamiento a fin de registrar la cuenta de almacenamiento con la implementación de Azure AD Domain Services asociada.
2. Asigne permisos de acceso para un recurso compartido a una identidad de Azure AD (usuario, grupo o entidad de servicio).
3. Configure los permisos NTFS a través de SMB para archivos y directorios.
4. Monte un recurso compartido de archivos de Azure desde una máquina virtual unida al dominio.

El diagrama siguiente ilustra el flujo de trabajo de extremo a extremo para habilitar la autenticación de Azure AD a través de SMB para Azure Files. 

![Diagrama que muestra Azure AD a través de SMB para el flujo de trabajo de Azure Files](media/storage-files-active-directory-enable/azure-active-directory-over-smb-workflow.png)

## <a name="prerequisites"></a>Requisitos previos 

1.  **Seleccione o cree un inquilino de Azure AD.**

    Puede usar un inquilino nuevo o uno existente para la autenticación de Azure AD a través de SMB. El inquilino y el recurso compartido de archivos a los que desea acceder deben asociarse con la misma suscripción.

    Para crear un nuevo inquilino de Azure AD, puede [agregar un inquilino de Azure AD y una suscripción de Azure AD](https://docs.microsoft.com/windows/client-management/mdm/add-an-azure-ad-tenant-and-azure-ad-subscription). Si ya tiene un inquilino de Azure AD, pero desea crear otro inquilino para usarlo con Azure Files, consulte [Obtención de un inquilino de Azure Active Directory](https://docs.microsoft.com/rest/api/datacatalog/create-an-azure-active-directory-tenant).

2.  **Habilite Azure AD Domain Services en el inquilino de Azure AD.**

    Para admitir la autenticación con las credenciales de Azure AD, debe habilitar Azure AD Domain Services para su inquilino de Azure AD. Si no es el administrador del inquilino de Azure AD, póngase en contacto con el administrador y siga las instrucciones paso a paso para [habilitar Azure Active Directory Domain Services mediante Azure Portal](../../active-directory-domain-services/active-directory-ds-getting-started.md).

    Suele tardar unos 15 minutos en completarse una implementación de Azure AD Domain Services. Confirme que el estado de mantenimiento de Azure AD Domain Services muestra **En ejecución**, con la sincronización de hash de contraseña habilitada, antes de continuar con el paso siguiente.

3.  **Unión a dominio de una máquina virtual de Azure con Azure AD Domain Services.**

    Para acceder a un recurso compartido de archivos mediante las credenciales de Azure AD desde una máquina virtual, la máquina virtual debe unirse al dominio con Azure AD Domain Services. Para más información sobre cómo unir a un dominio una máquina virtual, consulte [Unión de una máquina virtual de Windows Server a un dominio administrado](../../active-directory-domain-services/active-directory-ds-admin-guide-join-windows-vm-portal.md).

    > [!NOTE]
    > La autenticación de Azure AD a través de SMB con Azure Files solo se admite en las máquinas virtuales de Azure que se ejecutan en las versiones de sistemas operativos posteriores a Windows 7 o Windows Server 2008 R2.

4.  **Seleccione o cree un recurso compartido de archivos**.

    Seleccione un recurso compartido de archivos nuevo o existente que esté asociado con la misma suscripción que el inquilino de Azure AD. Para información acerca de cómo crear un nuevo recurso compartido de archivos, consulte [Creación de un recurso compartido de archivos en Azure Files](storage-how-to-create-file-share.md). 

    El inquilino de Azure AD debe implementarse en una región compatible con la versión preliminar de Azure AD a través de SMB. La versión preliminar está disponible en todas las regiones públicas, excepto para: Oeste de Estados Unidos, Oeste de EE.UU. 2, Centro y Sur de EE. UU, Este de Estados Unidos 2, Centro de EE. UU., Norte y Centro de EE. UU., Este de Australia, Europa Occidental, Europa del Norte.

    Para obtener un rendimiento óptimo, Microsoft recomienda que el recurso compartido de archivos esté en la misma región que la máquina virtual desde la que vaya a acceder al recurso compartido.

5.  **Compruebe la conectividad de Azure Files; para ello, monte los recursos compartidos de archivos de Azure con la clave de su cuenta de almacenamiento.**

    Para comprobar que el recurso compartido de archivos y de la máquina virtual están configurados correctamente, intente montar el recurso compartido de archivos con la clave de la cuenta de almacenamiento. Para más información, consulte [Montaje de un recurso compartido de archivos de Azure y acceso al recurso compartido en Windows](storage-how-to-use-files-windows.md).

## <a name="enable-azure-ad-authentication"></a>Habilitación de la autenticación de Azure AD

Después de haber completado los [requisitos previos](#prerequisites), puede habilitar la autenticación de Azure AD a través de SMB.

### <a name="step-1-enable-azure-ad-authentication-over-smb-for-your-storage-account"></a>Paso 1: Habilitar la autenticación de Azure AD a través de SMB para la cuenta de almacenamiento

Para habilitar la autenticación de Azure AD a través de SMB para Azure Files, puede establecer una propiedad en las cuentas de almacenamiento creadas después del 29 de agosto de 2018, mediante el proveedor de recursos de Azure Storage desde PowerShell o la CLI de Azure. No se permite la configuración de la propiedad en Azure Portal para la versión preliminar. 

Al establecer esta propiedad, registra la cuenta de almacenamiento con la implementación de Azure AD Domain Services asociada. A continuación, se habilita la autenticación de Azure AD a través de SMB para todos los recursos compartidos de archivos nuevos y existentes en la cuenta de almacenamiento. 

Tenga en cuenta que solo puede habilitar la autenticación de Azure AD a través de SMB después de haber implementado correctamente Azure AD Domain Services para su inquilino de Azure AD. Para más información, consulte los [requisitos previos](#prerequisites).

**PowerShell**

Para habilitar la autenticación de Azure AD a través de SMB, instale el módulo de PowerShell `AzureRM.Storage 6.0.0-preview`. Para más información acerca de cómo instalar PowerShell, consulte [Instalación de Azure PowerShell en Windows con PowerShellGet](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

A continuación, llame a [Set-AzureRmStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.storage/set-azurermstorageaccount) y establezca el parámetro **EnableAzureFilesAadIntegrationForSMB** en **true**. En el ejemplo siguiente, no olvide reemplazar los valores del marcador de posición por los suyos propios.

```powershell
# Create a new storage account
New-AzureRmStorageAccount -ResourceGroupName "<resource-group-name>" `
    -Name "<storage-account-name>" `
    -Location "<azure-region>" `
    -SkuName Standard_LRS `
    -Kind StorageV2 `
    -EnableAzureFilesAadIntegrationForSMB $true

# Update an existing storage account
# Supported for storage accounts created after August 29, 2018 only
Set-AzureRmStorageAccount -ResourceGroupName "<resource-group-name>" `
    -Name "<storage-account-name>" `
    -EnableAzureFilesAadIntegrationForSMB $true```
```

**CLI**

"Primero debe agregar la extensión de almacenamiento de la versión preliminar y, después, seguir el ejemplo a continuación. Reemplace los valores del marcador de posición por los suyos propios. Consulte este artículo para obtener más información acerca de cómo agregar extensiones de la CLI de Azure."

Para habilitar la autenticación de Azure AD a través de SMB desde la CLI de Azure 2.0, instale primero la extensión *storage-preview*:

```azurecli-interactive
az extension add --name storage-preview
```

A continuación, llame a [az storage account update](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-update) y establezca la propiedad `--file-aad` en **true**. En el ejemplo siguiente, no olvide reemplazar los valores del marcador de posición por los suyos propios.

```azurecli-interactive
# Create a new storage account
az storage account create -n <storage-account-name> -g <resource-group-name> --file-aad true

# Update an existing storage account
# Supported for storage accounts created after August 29, 2018 only
az storage account update -n <storage-account-name> -g <resource-group-name> --file-aad true
```

### <a name="step-2-assign-access-permissions-to-an-identity"></a>Paso 2: Asignar permisos de acceso a una identidad 

Para acceder a los recursos de Azure Files con las credenciales de Azure AD, una identidad (usuario, grupo o entidad de servicio) debe tener los permisos necesarios en los recursos compartidos. Las instrucciones paso a paso siguientes muestran cómo asignar permisos de lectura, escritura o eliminación para un recurso compartido de archivos a una identidad.

> [!IMPORTANT]
> El control administrativo total de un recurso compartido de archivos, incluida la capacidad para asignar un rol a una identidad, requiere usar la clave de la cuenta de almacenamiento. No se admite el control administrativo con credenciales de Azure AD. 

#### <a name="step-21-define-a-custom-role"></a>Paso 2.1: Definir un rol personalizado

Para conceder permisos en los recursos compartidos, defina un rol RBAC personalizado y asígnelo a una identidad, estableciendo el ámbito en un recurso compartido de archivo específico. Este proceso es similar a especificar los permisos de recurso compartido de Windows, donde se especifica el tipo de acceso que tiene un usuario determinado a un recurso compartido de archivos.  

Las plantillas que se muestran en las secciones siguientes proporcionan los permisos para leer o cambiar un recurso compartido de archivos. Para definir un rol personalizado, cree un archivo JSON y copie la plantilla adecuada en ese archivo. Para más información acerca de cómo definir roles personalizados de RBAC, consulte [Roles personalizados en Azure](../../role-based-access-control/custom-roles.md).

**Definición de roles para los permisos de cambio en los recursos compartidos**

La siguiente plantilla de rol personalizado proporciona permisos de cambio en los recursos compartidos, y conceden a una identidad acceso de eliminación, escritura y lectura en el recurso compartido.

```json
{
  "Name": "<Custom-Role-Name>",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows for read, write and delete access to Azure File Share",
  "Actions": [
    "*"
  ],
  "NotActions": [
      "Microsoft.Authorization/*/Delete",
    "Microsoft.Authorization/*/Write",
    "Microsoft.Authorization/elevateAccess/Action"
  ],
  "DataActions": [
    "*"
  ],
  "AssignableScopes": [
        "/subscriptions/<Subscription-ID>"
  ]
}
```

**Definición de roles para los permisos de lectura en los recursos compartidos**

La siguiente plantilla de rol personalizado proporciona permisos de lectura en los recursos compartidos y conceden a una identidad acceso de lectura en el recurso compartido.

```json
{
  "Name": "<Custom-Role-Name>",
  "Id": null,
  "IsCustom": true,
  "Description": "Allows for read access to Azure File Share",
  "Actions": [
    "*/read"
  ],
  "DataActions": [
    "*/read"
  ],
  "AssignableScopes": [
        "/subscriptions/<Subscription-ID>"
  ]
}
```

#### <a name="step-22-create-the-custom-role-and-assign-it-to-the-target-identity"></a>Paso 2.2: Crear el rol personalizado y asignarlo a la identidad de destino

A continuación, use PowerShell o la CLI de Azure para crear el rol y asignarlo a una identidad de Azure AD. 

**PowerShell**

Para habilitar la autenticación de Azure AD a través de SMB, instale el módulo de PowerShell `AzureRM.Storage 6.0.0-preview`. Para más información acerca de cómo instalar PowerShell, consulte [Instalación de Azure PowerShell en Windows con PowerShellGet](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

El siguiente comando de PowerShell crea un rol personalizado y lo asigna a una identidad de Azure AD, según el nombre de inicio de sesión. Para más información sobre la asignación de roles RBAC con PowerShell, consulte [Administración del acceso mediante RBAC y Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md).

Al ejecutar el siguiente script de ejemplo, no olvide reemplazar los valores de marcador de posición por los suyos propios.

```powershell
#Create a custom role based on the sample template above
New-AzureRmRoleDefinition -InputFile "<custom-role-def-json-path>"
#Get the name of the custom role
$FileShareContributorRole = Get-AzureRmRoleDefinition "<role-name>"
#Constrain the scope to the target file share
$scope = "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshare/<share-name>"
#Assign the custom role to the target identity with the specified scope.
New-AzureRmRoleAssignment -SignInName <user-principal-name> -RoleDefinitionName $FileShareContributorRole.Name -Scope $scope
```

**CLI**

El siguiente comando de la CLI 2.0 crea un rol personalizado y lo asigna a una identidad de Azure AD, según el nombre de inicio de sesión. Para más información sobre la asignación de roles RBAC con la CLI de Azure, consulte [Administración del acceso mediante RBAC y la CLI de Azure](../../role-based-access-control/role-assignments-cli.md). 

Al ejecutar el siguiente script de ejemplo, no olvide reemplazar los valores de marcador de posición por los suyos propios.

```azurecli-interactive
#Create a custom role based on the sample templates above
az role definition create --role-definition "<Custom-role-def-JSON-path>"
#List the custom roles
az role definition list --custom-role-only true --output json | jq '.[] | {"roleName":.roleName, "description":.description, "roleType":.roleType}'
#Assign the custom role to the target identity
az role assignment create --role "<custome-role-name>" --assignee <user-principal-name> --scope "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshare/<share-name>"
```

### <a name="step-3-configure-ntfs-permissions-over-smb"></a>Paso 3: Configurar los permisos NTFS a través de SMB 

Después de asignar los permisos en los recursos compartidos con RBAC, debe asignar los permisos NTFS adecuados en las raíces, los directorios o los archivos. Considere los permisos en los recursos compartidos como un selector de alto nivel que determina si un usuario puede acceder al recurso compartido, mientras que los permisos NTFS actúan en un nivel más pormenorizado para establecer qué operaciones puede realizar el usuario en los directorios o archivos. 

Azure Files admite el conjunto completo de permisos NTFS básicos y avanzados. Puede ver y configurar los permisos NTFS en los directorios y los archivos de un recurso compartido de archivos de Azure; para ello, monte el recurso compartido y, a continuación, ejecute el comando [icacls](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls) o [Set-ACL](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-acl) de Windows. 

> [!NOTE]
> La versión preliminar solo permite ver los permisos con el Explorador de archivos de Windows. Aún no se admiten los permisos de edición.

Para configurar los permisos NTFS con privilegios de superusuario, debe montar el recurso compartido con la clave de la cuenta de almacenamiento de la máquina virtual unida al dominio. Siga las instrucciones de la sección siguiente para montar un recurso compartido de archivos de Azure desde el símbolo del sistema y configurar los permisos NTFS en consecuencia.

Se admiten los siguientes conjuntos de permisos en el directorio raíz de un recurso compartido de archivos:

- BUILTIN\Administrators:(OI)(CI)(F)
- NT AUTHORITY\SYSTEM:(OI)(CI)(F)
- BUILTIN\Users:(RX)
- BUILTIN\Users:(OI)(CI)(IO)(GR,GE)
- NT AUTHORITY\Authenticated Users:(OI)(CI)(M)
- NT AUTHORITY\SYSTEM:(F)
- CREATOR OWNER:(OI)(CI)(IO)(F)

#### <a name="step-31-mount-an-azure-file-share-from-the-command-prompt"></a>Paso 3.1: Montar un recurso compartido de archivos de Azure desde el símbolo del sistema

Use el comando **net use** de Windows para montar el recurso compartido de archivos de Azure. No olvide reemplazar los valores del marcador de posición en el ejemplo por los suyos propios. Para más información sobre cómo montar los recursos compartidos de archivos, consulte [Montaje de un recurso compartido de archivos de Azure y acceso al recurso compartido en Windows](storage-how-to-use-files-windows.md).

```
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
```

#### <a name="step-32-configure-ntfs-permissions-with-icacls"></a>Paso 3.2: Configurar los permisos NTFS con icacls

Utilice el siguiente comando de Windows para conceder permisos completos para todos los directorios y archivos en el recurso compartido de archivos, incluido el directorio raíz. No olvide reemplazar los valores del marcador de posición en el ejemplo por los suyos propios.

```
icacls <mounted-drive-letter> /grant <user-email>:(f)
```

Para más información sobre cómo usar icacls para establecer los permisos NTFS y sobre los distintos tipos de permisos admitidos, consulte [la referencia de línea de comandos de icacls](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls).

### <a name="step-4-mount-an-azure-file-share-from-a-domain-joined-vm"></a>Paso 4: Montar un recurso compartido de archivos de Azure desde una máquina virtual unida al dominio 

Ahora está preparado para comprobar que ha completado los pasos anteriores correctamente usando sus credenciales de Azure AD para acceder a un recurso compartido de archivos de Azure desde una máquina virtual unida al dominio. En primer lugar, inicie sesión en la máquina virtual con la identidad de Azure AD a la que se hayan concedido permisos, tal como se muestra en la siguiente imagen.

![Captura de pantalla que muestra la pantalla Inicio de sesión de Azure AD para la autenticación de usuario](media/storage-files-active-directory-enable/azure-active-directory-authentication-dialog.png)

Después, use el comando siguiente para montar el recurso compartido de archivos de Azure. Reemplace los valores del marcador de posición por los suyos propios. Dado que ya se ha autenticado, no tiene que proporcionar la clave de la cuenta de almacenamiento ni el nombre de usuario y la contraseña de Azure AD. Azure AD a través de SMB admite el inicio de sesión único con las credenciales de Azure AD.

```
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name>
```

Ahora ha habilitado correctamente la autenticación de Azure AD a través de SMB y ha asignado un rol personalizado que proporciona acceso a un recurso compartido de archivos a una identidad de Azure AD. Para conceder acceso a su recurso compartido de archivos a usuarios adicionales, siga las instrucciones proporcionadas en el paso 2.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure Files y el uso de Azure AD a través de SMB, consulte estos recursos:

- [Introducción a Azure Files](storage-files-introduction.md)
- [Introducción a la autenticación de Azure Active Directory a través de SMB para Azure Files (versión preliminar)](storage-files-active-directory-overview.md)
- [P+F](storage-files-faq.md)