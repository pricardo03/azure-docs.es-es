---
title: 'Habilitación de la autenticación de Azure Active Directory a través de SMB para Azure Files (versión preliminar): Azure Storage'
description: Aprenda a habilitar la autenticación basada en identidades a través de SMB (Bloque de mensajes del servidor) (versión preliminar) para Azure Files mediante Azure Active Directory (Azure AD) Domain Services. Las máquinas virtuales Windows unidas al dominio pueden entonces acceder a los recursos compartidos de archivos de Azure con las credenciales de Azure AD.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 07/05/2019
ms.author: rogarana
ms.openlocfilehash: cd4c952caa336f2602d3c30e0db3e10ebee59cb9
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67696121"
---
# <a name="enable-azure-active-directory-domain-service-authentication-over-smb-for-azure-files-preview"></a>Habilitación de la autenticación del servicio Dominio de Active Directory mediante SMB para Azure Files (versión preliminar)
[!INCLUDE [storage-files-aad-auth-include](../../../includes/storage-files-aad-auth-include.md)]

Para obtener información general de la autenticación de Azure AD a través de SMB de Azure Files, consulte [Introducción a la autenticación de Azure Active Directory a través de SMB para Azure Files (versión preliminar)](storage-files-active-directory-overview.md).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="overview-of-the-workflow"></a>Información general del flujo de trabajo
Antes de habilitar la autenticación de Azure AD DS a través de SMB para Azure Files, compruebe que los entornos de Azure Storage y Azure AD se han configurado correctamente. Se recomienda que revise los [requisitos previos](#prerequisites) para asegurarse de que ha realizado todos los pasos necesarios. 

A continuación, conceda acceso a los recursos de Azure Files con las credenciales de Azure AD; para ello, siga estos pasos: 

1. Habilite la autenticación de Azure AD DS a través de SMB para la cuenta de almacenamiento a fin de registrar dicha cuenta con la implementación de Azure AD Domain Services asociada.
2. Asigne permisos de acceso para un recurso compartido a una identidad de Azure AD (usuario, grupo o entidad de servicio).
3. Configure los permisos NTFS a través de SMB para archivos y directorios.
4. Monte un recurso compartido de archivos de Azure desde una máquina virtual unida al dominio.

En el diagrama siguiente se ilustra el flujo de trabajo de un extremo a otro para habilitar la autenticación de Azure AD a través de SMB para Azure Files. 

![Diagrama que muestra Azure AD a través de SMB para el flujo de trabajo de Azure Files](media/storage-files-active-directory-enable/azure-active-directory-over-smb-workflow.png)

## <a name="prerequisites"></a>Requisitos previos 

Antes de habilitar Azure AD a través de SMB para Azure Files, asegúrese de haber completado los requisitos previos siguientes:

1.  **Seleccione o cree un inquilino de Azure AD.**

    Puede usar un inquilino nuevo o uno existente para la autenticación de Azure AD a través de SMB. El inquilino y el recurso compartido de archivos a los que desea acceder deben asociarse con la misma suscripción.

    Para crear un nuevo inquilino de Azure AD, puede [agregar un inquilino de Azure AD y una suscripción de Azure AD](https://docs.microsoft.com/windows/client-management/mdm/add-an-azure-ad-tenant-and-azure-ad-subscription). Si ya tiene un inquilino de Azure AD, pero desea crear otro inquilino para usarlo con Azure Files, consulte [Obtención de un inquilino de Azure Active Directory](https://docs.microsoft.com/rest/api/datacatalog/create-an-azure-active-directory-tenant).

2.  **Habilite Azure AD Domain Services en el inquilino de Azure AD.**

    Para admitir la autenticación con las credenciales de Azure AD, debe habilitar Azure AD Domain Services para su inquilino de Azure AD. Si no es el administrador del inquilino de Azure AD, póngase en contacto con el administrador y siga las instrucciones paso a paso para [habilitar Azure Active Directory Domain Services mediante Azure Portal](../../active-directory-domain-services/create-instance.md).

    Suele tardar unos 15 minutos en completarse una implementación de Azure AD Domain Services. Confirme que el estado de mantenimiento de Azure AD Domain Services muestra **En ejecución**, con la sincronización de hash de contraseña habilitada, antes de continuar con el paso siguiente.

3.  **Unión a dominio de una máquina virtual de Azure con Azure AD Domain Services.**

    Para acceder a un recurso compartido de archivos mediante las credenciales de Azure AD desde una máquina virtual, la máquina virtual debe unirse al dominio con Azure AD Domain Services. Para más información sobre cómo unir a un dominio una máquina virtual, consulte [Unión de una máquina virtual de Windows Server a un dominio administrado](../../active-directory-domain-services/join-windows-vm.md).

    > [!NOTE]
    > La autenticación de Azure AD DS a través de SMB con Azure Files solo se admite en máquinas virtuales de Azure que se ejecutan en versiones del sistema operativo posteriores a Windows 7 o Windows Server 2008 R2.

4.  **Seleccione o cree un recurso compartido de archivos**.

    Seleccione un recurso compartido de archivos nuevo o existente que esté asociado con la misma suscripción que el inquilino de Azure AD. Para información acerca de cómo crear un nuevo recurso compartido de archivos, consulte [Creación de un recurso compartido de archivos en Azure Files](storage-how-to-create-file-share.md). 
    Para obtener un rendimiento óptimo, Microsoft recomienda que el recurso compartido de archivos esté en la misma región que la máquina virtual desde la que vaya a acceder al recurso compartido.

5.  **Compruebe la conectividad de Azure Files; para ello, monte los recursos compartidos de archivos de Azure con la clave de su cuenta de almacenamiento.**

    Para comprobar que el recurso compartido de archivos y de la máquina virtual están configurados correctamente, intente montar el recurso compartido de archivos con la clave de la cuenta de almacenamiento. Para más información, consulte [Montaje de un recurso compartido de archivos de Azure y acceso al recurso compartido en Windows](storage-how-to-use-files-windows.md).

## <a name="enable-azure-ad-ds-authentication-for-your-account"></a>Habilitación de la autenticación de Azure AD DS para la cuenta

Para habilitar la autenticación de Azure AD DS a través de SMB para Azure Files, puede establecer una propiedad en las cuentas de almacenamiento creadas después del 24 de septiembre de 2018, mediante Azure Portal, Azure PowerShell o la CLI de Azure. Al establecer esta propiedad, registra la cuenta de almacenamiento con la implementación de Azure AD Domain Services asociada. La autenticación de Azure AD DS a través de SMB se habilita entonces para todos los recursos compartidos de archivos nuevos y existentes de la cuenta de almacenamiento. 

Tenga en cuenta que solo puede habilitar la autenticación de Azure AD DS a través de SMB después de haber implementado correctamente Azure AD Domain Services en el inquilino de Azure AD. Para más información, consulte los [requisitos previos](#prerequisites).

### <a name="azure-portal"></a>Portal de Azure

Para habilitar la autenticación de Azure AD DS a través de SMB mediante [Azure Portal](https://portal.azure.com), siga estos pasos:

1. En Azure Portal, vaya a la cuenta de almacenamiento existente o [cree una nueva](../common/storage-quickstart-create-account.md).
2. En la sección **Configuración**, haga clic en **Configuración**.
3. Habilite la **autenticación de Azure Active Directory para Azure Files (versión preliminar)** .

En la imagen siguiente se muestra cómo habilitar la autenticación de Azure AD a través de SMB para la cuenta de almacenamiento.

![Habilitación de la autenticación de Azure AD a través de SMB en Azure Portal](media/storage-files-active-directory-enable/portal-enable-active-directory-over-smb.png)
  
### <a name="powershell"></a>PowerShell  

Para habilitar la autenticación de Azure AD DS a través de SMB con Azure PowerShell: En primer lugar, instale el módulo Az más reciente (2.4 o posterior) o el módulo Az.Storage (1.5 o posterior). Para más información sobre la instalación de PowerShell, consulte [Instalación de Azure PowerShell en Windows con PowerShellGet](https://docs.microsoft.com/powershell/azure/install-Az-ps):

```powershell
Install-Module -Name Az.Storage -AllowPrerelease -Force -AllowClobber
```

A continuación, cree una cuenta de almacenamiento, llame a [Set-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/set-azstorageaccount) y establezca el parámetro **EnableAzureActiveDirectoryDomainServicesForFile** en **true**. En el ejemplo siguiente, no olvide reemplazar los valores del marcador de posición por los suyos propios. (Si usaba el módulo de vista previa anterior, el parámetro para la habilitación de características es **EnableAzureFilesAadIntegrationForSMB**).

```powershell
# Create a new storage account
New-AzStorageAccount -ResourceGroupName "<resource-group-name>" `
    -Name "<storage-account-name>" `
    -Location "<azure-region>" `
    -SkuName Standard_LRS `
    -Kind StorageV2 `
    -EnableAzureActiveDirectoryDomainServicesForFile $true
```
Para habilitar esta característica en cuentas de almacenamiento existentes, use el siguiente comando:

```powershell
# Update a storage account
Set-AzStorageAccount -ResourceGroupName "<resource-group-name>" `
    -Name "<storage-account-name>" `
    -EnableAzureActiveDirectoryDomainServicesForFile $true
```


### <a name="azure-cli"></a>CLI de Azure

Para habilitar la autenticación de Azure AD a través de SMB desde la CLI de Azure 2.0, instale primero la extensión `storage-preview`:

```cli-interactive
az extension add --name storage-preview
```
  
A continuación, cree una nueva cuenta de almacenamiento y, después, llame a [az storage account update](https://docs.microsoft.com/cli/azure/storage/account#az-storage-account-update) y establezca la propiedad `--file-aad` en **true**. En el ejemplo siguiente, no olvide reemplazar los valores del marcador de posición por los suyos propios.

```azurecli-interactive
# Create a new storage account
az storage account create -n <storage-account-name> -g <resource-group-name> --file-aad true
```

## <a name="assign-access-permissions-to-an-identity"></a>Asignar permisos de acceso a una identidad 

Para acceder a los recursos de Azure Files con las credenciales de Azure AD, una identidad (usuario, grupo o entidad de servicio) debe tener los permisos necesarios en los recursos compartidos. Este proceso es similar a especificar los permisos de recurso compartido de Windows, donde se especifica el tipo de acceso que tiene un usuario determinado a un recurso compartido de archivos. Las instrucciones de esta sección muestran cómo asignar permisos de lectura, escritura o eliminación para un recurso compartido de archivos a una identidad.

Se han incorporado dos roles integrados de Azure para conceder permisos de nivel de recurso compartido a los usuarios: lector de recursos compartidos de SMB para los datos del archivo de almacenamiento y colaborador de recursos compartidos de SMB para los datos del archivo de almacenamiento. 

- El **lector de recursos compartidos de SMB para los datos del archivo de almacenamiento** permite el acceso de lectura a los recursos compartidos de archivos de Azure Storage a través de SMB.
- El **colaborador de recursos compartidos de SMB para los datos del archivo de almacenamiento** permite el acceso de lectura, escritura y eliminación en los recursos compartidos de archivos de Azure Storage a través de SMB.

> [!IMPORTANT]
> El control administrativo total de un recurso compartido de archivos, incluida la capacidad para asignar un rol a una identidad, requiere usar la clave de la cuenta de almacenamiento. El control administrativo no puede realizarse con credenciales de Azure AD. 

Puede usar Azure Portal, PowerShell o la CLI de Azure para asignar los roles integrados a la identidad de Azure AD de un usuario a fin de conceder permisos de nivel de recurso compartido. 

#### <a name="azure-portal"></a>Portal de Azure
Para asignar un rol de RBAC a una identidad de Azure AD mediante [Azure Portal](https://portal.azure.com), siga estos pasos:

1. En Azure Portal, vaya al recurso compartido de archivos o [cree un recurso compartido de archivos en Azure Files](storage-how-to-create-file-share.md).
2. Seleccione **Access Control (IAM)** .
3. Seleccione **Agregar una asignación de roles**.
4. En la hoja **Agregar asignación de roles**, seleccione el rol integrado adecuado (lector o colaborador de recursos compartidos de SMB para datos del archivo de almacenamiento) en la lista desplegable **Rol**. Mantenga **Asignar acceso a** en el valor predeterminado: "Usuario, grupo o entidad de servicio de Azure AD" y seleccione la identidad de Azure AD de destino por nombre o dirección de correo electrónico. 
5. Por último, seleccione **Guardar** para completar la operación de asignación de roles.

#### <a name="powershell"></a>PowerShell

El siguiente comando de PowerShell muestra cómo asignar un rol de RBAC a una identidad de Azure AD, según el nombre de inicio de sesión. Para más información sobre la asignación de roles RBAC con PowerShell, consulte [Administración del acceso mediante RBAC y Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md).

Al ejecutar el siguiente script de ejemplo, no olvide reemplazar los valores de marcador de posición, incluidos los corchetes, por los suyos propios.

```powershell
#Get the name of the custom role
$FileShareContributorRole = Get-AzRoleDefinition "<role-name>" #Use one of the built-in roles: Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor
#Constrain the scope to the target file share
$scope = "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshare/<share-name>"
#Assign the custom role to the target identity with the specified scope.
New-AzRoleAssignment -SignInName <user-principal-name> -RoleDefinitionName $FileShareContributorRole.Name -Scope $scope
```

#### <a name="cli"></a>CLI
  
El siguiente comando de la CLI 2.0 muestra cómo asignar un rol de RBAC a una identidad de Azure AD, según el nombre de inicio de sesión. Para más información sobre la asignación de roles RBAC con la CLI de Azure, consulte [Administración del acceso mediante RBAC y la CLI de Azure](../../role-based-access-control/role-assignments-cli.md). 

Al ejecutar el siguiente script de ejemplo, no olvide reemplazar los valores de marcador de posición, incluidos los corchetes, por los suyos propios.

```azurecli-interactive
#Assign the built-in role to the target identity: Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor
az role assignment create --role "<role-name>" --assignee <user-principal-name> --scope "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshare/<share-name>"
```

## <a name="configure-ntfs-permissions-over-smb"></a>Configurar los permisos NTFS a través de SMB 
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

### <a name="mount-a-file-share-from-the-command-prompt"></a>Montar un recurso compartido de archivos de Azure desde el símbolo del sistema

Use el comando **net use** de Windows para montar el recurso compartido de archivos de Azure. No olvide reemplazar los valores del marcador de posición en el ejemplo por los propios. Para más información sobre cómo montar los recursos compartidos de archivos, consulte [Montaje de un recurso compartido de archivos de Azure y acceso al recurso compartido en Windows](storage-how-to-use-files-windows.md).

```
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
```

### <a name="configure-ntfs-permissions-with-icacls"></a>Configurar los permisos NTFS con icacls
Utilice el siguiente comando de Windows para conceder permisos completos para todos los directorios y archivos en el recurso compartido de archivos, incluido el directorio raíz. No olvide reemplazar los valores del marcador de posición que se muestran entre paréntesis en el ejemplo por los suyos propios.

```
icacls <mounted-drive-letter>: /grant <user-email>:(f)
```

Para más información sobre cómo usar icacls para establecer los permisos NTFS y sobre los distintos tipos de permisos admitidos, consulte [la referencia de línea de comandos de icacls](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls).

## <a name="mount-a-file-share-from-a-domain-joined-vm"></a>Montar un recurso compartido de archivos desde una máquina virtual unida al dominio 

Ahora está preparado para comprobar que ha completado los pasos anteriores correctamente usando sus credenciales de Azure AD para acceder a un recurso compartido de archivos de Azure desde una máquina virtual unida al dominio. En primer lugar, inicie sesión en la máquina virtual con la identidad de Azure AD a la que se hayan concedido permisos, tal como se muestra en la siguiente imagen.

![Captura de pantalla que muestra la pantalla Inicio de sesión de Azure AD para la autenticación de usuario](media/storage-files-active-directory-enable/azure-active-directory-authentication-dialog.png)

Después, use el comando siguiente para montar el recurso compartido de archivos de Azure. Reemplace los valores del marcador de posición por los propios. Dado que ya se ha autenticado, no tiene que proporcionar la clave de la cuenta de almacenamiento ni el nombre de usuario y la contraseña de Azure AD. Azure AD a través de SMB admite el inicio de sesión único con las credenciales de Azure AD.

```
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name>
```

Ahora ha habilitado correctamente la autenticación de Azure AD a través de SMB y ha asignado un rol personalizado que proporciona acceso a un recurso compartido de archivos a una identidad de Azure AD. Para conceder acceso a su recurso compartido de archivos a usuarios adicionales, siga las instrucciones proporcionadas en los pasos 2 y 3.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure Files y el uso de Azure AD a través de SMB, consulte estos recursos:

- [Introducción a Azure Files](storage-files-introduction.md)
- [Introducción a la autenticación de Azure Active Directory a través de SMB para Azure Files (versión preliminar)](storage-files-active-directory-overview.md)
- [P+F](storage-files-faq.md)
