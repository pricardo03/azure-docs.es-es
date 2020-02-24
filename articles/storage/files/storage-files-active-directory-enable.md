---
title: Uso de Azure AD Domain Services para autorizar el acceso a los datos de archivo a través de SMB
description: Aprenda a habilitar la autenticación basada en identidades a través del Bloque de mensajes del servidor (SMB) para Azure Files mediante Azure Active Directory Domain Services. Las máquinas virtuales Windows unidas a un dominio pueden acceder entonces a los recursos compartidos de archivos de Azure con las credenciales de Azure AD.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 01/06/2020
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 06ff14b23057755a643e5a57fbaf711798cca00e
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2020
ms.locfileid: "77210489"
---
# <a name="enable-azure-active-directory-domain-services-authentication-on-azure-files"></a>Habilitación de la autenticación de Azure Active Directory Domain Services en Azure Files

[!INCLUDE [storage-files-aad-auth-include](../../../includes/storage-files-aad-auth-include.md)]

Para obtener información general sobre la autenticación basada en identidad admitida en Azure Files, consulte [Introducción a la autenticación de Azure Active Directory a través de SMB para Azure Files](storage-files-active-directory-overview.md). Este artículo se centra en cómo habilitar la autenticación con Azure Active Directory Domain Services (Azure AD DS) en Azure Files. 
[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="overview-of-the-workflow"></a>Información general del flujo de trabajo

Antes de habilitar la autenticación de Azure AD DS a través de SMB para Azure Files, compruebe que los entornos de Azure Storage y Azure AD se han configurado correctamente. Se recomienda que revise los [requisitos previos](#prerequisites) para asegurarse de que ha completado todos los pasos necesarios.

A continuación, conceda acceso a los recursos de Azure Files con las credenciales de Azure AD; para ello, siga estos pasos:

1. Habilite la autenticación de Azure AD DS a través de SMB en la cuenta de almacenamiento a fin de registrarla con la implementación de Azure AD DS asociada.
2. Asigne permisos de acceso para un recurso compartido a una identidad de Azure AD (usuario, grupo o entidad de servicio).
3. Configure los permisos NTFS a través de SMB para archivos y directorios.
4. Monte un recurso compartido de archivos de Azure desde una máquina virtual unida al dominio.

En el diagrama siguiente se ilustra el flujo de trabajo de un extremo a otro para habilitar la autenticación de Azure AD DS a través de SMB para Azure Files.

![Diagrama que muestra Azure AD a través de SMB para el flujo de trabajo de Azure Files](media/storage-files-active-directory-enable/azure-active-directory-over-smb-workflow.png)

## <a name="prerequisites"></a>Prerrequisitos

Antes de habilitar Azure AD a través de SMB para Azure Files, asegúrese de haber completado los requisitos previos siguientes:

1.  **Seleccione o cree un inquilino de Azure AD.**

    Puede usar un inquilino nuevo o uno existente para la autenticación de Azure AD a través de SMB. El inquilino y el recurso compartido de archivos a los que desea acceder deben asociarse con la misma suscripción.

    Para crear un nuevo inquilino de Azure AD, puede [agregar un inquilino de Azure AD y una suscripción de Azure AD](https://docs.microsoft.com/windows/client-management/mdm/add-an-azure-ad-tenant-and-azure-ad-subscription). Si ya tiene un inquilino de Azure AD, pero desea crear otro inquilino para usarlo con Azure Files, consulte [Obtención de un inquilino de Azure Active Directory](https://docs.microsoft.com/rest/api/datacatalog/create-an-azure-active-directory-tenant).

2.  **Habilite Azure AD Domain Services en el inquilino de Azure AD.**

    Para admitir la autenticación con las credenciales de Azure AD, debe habilitar Azure AD Domain Services para su inquilino de Azure AD. Si no es el administrador del inquilino de Azure AD, póngase en contacto con el administrador y siga las instrucciones paso a paso para [habilitar Azure Active Directory Domain Services mediante Azure Portal](../../active-directory-domain-services/tutorial-create-instance.md).

    Una implementación de Azure AD DS suele tardar unos 15 minutos en completarse. Confirme que el estado de mantenimiento de Azure AD muestra **En ejecución**, con la sincronización de hash de contraseña habilitada, antes de continuar con el paso siguiente.

3.  **Unión al dominio de una máquina virtual de Azure con Azure AD DS.**

    Para acceder a un recurso compartido de archivos mediante las credenciales de Azure AD desde una máquina virtual, esta debe unirse a un dominio con Azure AD DS. Para más información sobre cómo unir a un dominio una máquina virtual, consulte [Unión de una máquina virtual de Windows Server a un dominio administrado](../../active-directory-domain-services/join-windows-vm.md).

    > [!NOTE]
    > La autenticación de Azure AD DS a través de SMB con Azure Files solo se admite en máquinas virtuales de Azure que se ejecutan en versiones del sistema operativo posteriores a Windows 7 o Windows Server 2008 R2.

4.  **Seleccione o cree un recurso compartido de archivos**.

    Seleccione un recurso compartido de archivos nuevo o existente que esté asociado con la misma suscripción que el inquilino de Azure AD. Para información acerca de cómo crear un nuevo recurso compartido de archivos, consulte [Creación de un recurso compartido de archivos en Azure Files](storage-how-to-create-file-share.md).
    Para obtener un rendimiento óptimo, se recomienda que el recurso compartido de archivos esté en la misma región que la máquina virtual desde la que vaya a acceder al recurso compartido.

5.  **Compruebe la conectividad de Azure Files; para ello, monte los recursos compartidos de archivos de Azure con la clave de su cuenta de almacenamiento.**

    Para comprobar que el recurso compartido de archivos y de la máquina virtual están configurados correctamente, intente montar el recurso compartido de archivos con la clave de la cuenta de almacenamiento. Para más información, consulte [Montaje de un recurso compartido de archivos de Azure y acceso al recurso compartido en Windows](storage-how-to-use-files-windows.md).

## <a name="enable-azure-ad-ds-authentication-for-your-account"></a>Habilitación de la autenticación de Azure AD DS para la cuenta

Para habilitar la autenticación de Azure AD DS a través de SMB para Azure Files, puede establecer una propiedad en las cuentas de almacenamiento mediante Azure Portal, Azure PowerShell o la CLI de Azure. Al establecer esta propiedad "une a un dominio" implícitamente la cuenta de almacenamiento con la implementación de Azure AD DS asociada. La autenticación de Azure AD DS a través de SMB se habilita entonces para todos los recursos compartidos de archivos nuevos y existentes de la cuenta de almacenamiento.

Tenga en cuenta que solo puede habilitar la autenticación de Azure AD DS a través de SMB después de haber implementado correctamente Azure AD DS en el inquilino de Azure AD. Para más información, consulte la sección los [requisitos previos](#prerequisites).

### <a name="azure-portal"></a>Portal de Azure

Para habilitar la autenticación de Azure AD DS a través de SMB mediante [Azure Portal](https://portal.azure.com), siga estos pasos:

1. En Azure Portal, vaya a la cuenta de almacenamiento existente o [cree una](../common/storage-account-create.md).
2. En la sección **Configuración**, haga clic en **Configuración**.
3. Seleccione **Azure Active Directory Domain Services (Azure AD DS)** en la lista desplegable del **servicio de directorio basado en identidad para la autenticación de archivos de Azure**.

En la imagen siguiente se muestra cómo habilitar la autenticación de Azure AD DS a través de SMB para la cuenta de almacenamiento.

![Habilitación de la autenticación de Azure AD a través de SMB en Azure Portal](media/storage-files-active-directory-enable/portal-enable-active-directory-over-smb.png)

### <a name="powershell"></a>PowerShell  

Para habilitar la autenticación de Azure AD DS a través de SMB con Azure PowerShell, instale el módulo Az más reciente (2.4 o posterior) o el módulo Az.Storage (1.5 o posterior). Para más información sobre cómo instalar PowerShell, consulte [Instalación de Azure PowerShell en Windows con PowerShellGet](https://docs.microsoft.com/powershell/azure/install-Az-ps):

Para crear una cuenta de almacenamiento, llame a [New-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/New-azStorageAccount?view=azps-2.5.0) y establezca el parámetro **EnableAzureActiveDirectoryDomainServicesForFile** en **true**. En el ejemplo siguiente, no olvide reemplazar los valores de marcador de posición por los suyos propios. (Si ha estado usando el módulo de versión preliminar anterior, el parámetro para la habilitación de características es **EnableAzureFilesAadIntegrationForSMB**).

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


### <a name="azure-cli"></a>Azure CLI

Para habilitar la autenticación de Azure AD mediante SMB con la CLI de Azure, instale la versión más reciente de la CLI (versión 2.0.70 o posterior). Para más información sobre cómo instalar la CLI de Azure, consulte [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

Para crear una cuenta de almacenamiento, llame a [az storage account create](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-create) y establezca la propiedad `--enable-files-aadds` en **true**. En el ejemplo siguiente, no olvide reemplazar los valores de marcador de posición por los suyos propios. (Si ha estado usando el módulo de versión preliminar anterior, el parámetro para la habilitación de características es **file-aad**).

```azurecli-interactive
# Create a new storage account
az storage account create -n <storage-account-name> -g <resource-group-name> --enable-files-aadds $true
```

Para habilitar esta característica en cuentas de almacenamiento existentes, use el siguiente comando:

```azurecli-interactive
# Update a new storage account
az storage account update -n <storage-account-name> -g <resource-group-name> --enable-files-aadds $true
```


## <a name="assign-access-permissions-to-an-identity"></a>Asignar permisos de acceso a una identidad

Para acceder a los recursos de Azure Files con las credenciales de Azure AD, una identidad (usuario, grupo o entidad de servicio) debe tener los permisos necesarios en el nivel de recurso compartido. Este proceso es similar a especificar los permisos de recurso compartido de Windows, donde se especifica el tipo de acceso que tiene un usuario determinado a un recurso compartido de archivos. Las instrucciones de esta sección muestran cómo asignar permisos de lectura, escritura o eliminación para un recurso compartido de archivos a una identidad.

Se han incorporado dos roles integrados de Azure para conceder permisos de nivel de recurso compartido a los usuarios:

- El **lector de recursos compartidos de SMB para los datos del archivo de almacenamiento** permite el acceso de lectura en los recursos compartidos de archivos de Azure Storage a través de SMB.
- El **colaborador de recursos compartidos de SMB para los datos del archivo de almacenamiento** permite el acceso de lectura, escritura y eliminación en los recursos compartidos de archivos de Azure Storage a través de SMB.
- El **colaborador con privilegios elevados de recursos compartidos de SMB para los datos del archivo de almacenamiento** permite el acceso de lectura, escritura y eliminación en los recursos compartidos de archivos de Azure Storage a través de SMB.

> [!IMPORTANT]
> El control administrativo total de un recurso compartido de archivos, incluida la capacidad para asignar un rol a una identidad, requiere usar la clave de la cuenta de almacenamiento. El control administrativo no puede realizarse con credenciales de Azure AD.

Puede usar Azure Portal, PowerShell o la CLI de Azure para asignar los roles integrados a la identidad de Azure AD de un usuario a fin de conceder permisos de nivel de recurso compartido.

#### <a name="azure-portal"></a>Portal de Azure
Para asignar un rol de RBAC a una identidad de Azure AD mediante [Azure Portal](https://portal.azure.com), siga estos pasos:

1. En Azure Portal, vaya al recurso compartido de archivos o [cree un recurso compartido de archivos en Azure Files](storage-how-to-create-file-share.md).
2. Seleccione **Access Control (IAM)** .
3. Seleccione **Agregar una asignación de roles**.
4. En la hoja **Agregar asignación de roles**, seleccione el rol integrado adecuado (lector o colaborador de recursos compartidos de SMB para datos del archivo de almacenamiento) en la lista desplegable **Rol**. Mantenga la opción **Asignar acceso a** en la configuración predeterminada: **Usuario, grupo o entidad de servicio de Azure AD**. Seleccione la identidad de Azure AD de destino por nombre o dirección de correo electrónico.
5. Seleccione **Guardar** para completar la operación de asignación de roles.

#### <a name="powershell"></a>PowerShell

El siguiente comando de PowerShell muestra cómo asignar un rol de RBAC a una identidad de Azure AD, según el nombre de inicio de sesión. Para más información sobre la asignación de roles RBAC con PowerShell, consulte [Administración del acceso mediante RBAC y Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md).

Antes de ejecutar el siguiente script de ejemplo, no olvide reemplazar los valores de marcador de posición, incluidos los corchetes, por los suyos propios.

```powershell
#Get the name of the custom role
$FileShareContributorRole = Get-AzRoleDefinition "<role-name>" #Use one of the built-in roles: Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor, Storage File Data SMB Share Elevated Contributor
#Constrain the scope to the target file share
$scope = "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshares/<share-name>"
#Assign the custom role to the target identity with the specified scope.
New-AzRoleAssignment -SignInName <user-principal-name> -RoleDefinitionName $FileShareContributorRole.Name -Scope $scope
```

#### <a name="cli"></a>CLI

El siguiente comando de la CLI 2.0 muestra cómo asignar un rol de RBAC a una identidad de Azure AD, según el nombre de inicio de sesión. Para más información sobre la asignación de roles de RBAC con la CLI de Azure, consulte [Administración del acceso mediante RBAC y la CLI de Azure](../../role-based-access-control/role-assignments-cli.md).

Antes de ejecutar el siguiente script de ejemplo, no olvide reemplazar los valores de marcador de posición, incluidos los corchetes, por los suyos propios.

```azurecli-interactive
#Assign the built-in role to the target identity: Storage File Data SMB Share Reader, Storage File Data SMB Share Contributor, Storage File Data SMB Share Elevated Contributor
az role assignment create --role "<role-name>" --assignee <user-principal-name> --scope "/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.Storage/storageAccounts/<storage-account>/fileServices/default/fileshares/<share-name>"
```

## <a name="configure-ntfs-permissions-over-smb"></a>Configurar los permisos NTFS a través de SMB
Después de asignar los permisos en los recursos compartidos con RBAC, debe asignar los permisos NTFS adecuados en las raíces, los directorios o los archivos. Considere los permisos de nivel de recurso compartido como el equipo selector de alto nivel que determina si un usuario puede acceder al recurso compartido. Mientras, los permisos NTFS actúan en un nivel más granular para determinar qué operaciones puede realizar el usuario en el directorio o archivo.

Azure Files admite el conjunto completo de permisos NTFS básicos y avanzados. Puede ver y configurar los permisos NTFS en los directorios y los archivos de un recurso compartido de archivos de Azure; para ello, monte el recurso compartido y, a continuación, utilice el Explorador de archivos de Windows o ejecute el comando [icacls](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls) o [Set-ACL](https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-acl) de Windows.

Para configurar NTFS con permisos de superusuario, debe montar el recurso compartido con la clave de la cuenta de almacenamiento de la máquina virtual unida al dominio. Siga las instrucciones de la sección siguiente para montar un recurso compartido de archivos de Azure desde el símbolo del sistema y para configurar los permisos NTFS en consecuencia.

Se admiten los siguientes conjuntos de permisos en el directorio raíz de un recurso compartido de archivos:

- BUILTIN\Administrators:(OI)(CI)(F)
- NT AUTHORITY\SYSTEM:(OI)(CI)(F)
- BUILTIN\Users:(RX)
- BUILTIN\Users:(OI)(CI)(IO)(GR,GE)
- NT AUTHORITY\Authenticated Users:(OI)(CI)(M)
- NT AUTHORITY\SYSTEM:(F)
- CREATOR OWNER:(OI)(CI)(IO)(F)

### <a name="mount-a-file-share-from-the-command-prompt"></a>Montar un recurso compartido de archivos de Azure desde el símbolo del sistema

Use el comando **net use** de Windows para montar el recurso compartido de archivos de Azure. No olvide reemplazar los valores del marcador de posición del ejemplo por los suyos propios. Para más información sobre cómo montar los recursos compartidos de archivos, consulte [Montaje de un recurso compartido de archivos de Azure y acceso al recurso compartido en Windows](storage-how-to-use-files-windows.md).

```
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name> <storage-account-key> /user:Azure\<storage-account-name>
```
### <a name="configure-ntfs-permissions-with-windows-file-explorer"></a>Configuración de permisos NTFS con el Explorador de archivos de Windows
Utilice el Explorador de archivos de Windows para conceder permisos completos para todos los directorios y archivos en el recurso compartido de archivos, incluido el directorio raíz.

1. Abra el Explorador de archivos de Windows y haga clic con el botón derecho en el archivo o directorio y seleccione **Propiedades**.
2. Haga clic en la pestaña **Seguridad**.
3. Haga clic en el botón **Editar...** para cambiar permisos.
4. Puede cambiar el permiso de los usuarios existentes o hacer clic en **Agregar...** para conceder permisos a nuevos usuarios.
5. En la ventana del símbolo del sistema para agregar nuevos usuarios, escriba el nombre de usuario de destino al que desea conceder el permiso en el cuadro **Escriba los nombres de objeto que desea seleccionar** y haga clic en **Comprobar nombres** para buscar el nombre UPN completo del usuario de destino.
7.  Haga clic en **Aceptar**
8.  En la pestaña Seguridad, seleccione todos los permisos que desea conceder al usuario que acaba de agregar.
9.  Haga clic en **Aplicar**

### <a name="configure-ntfs-permissions-with-icacls"></a>Configurar los permisos NTFS con icacls
Utilice el siguiente comando de Windows para conceder permisos completos para todos los directorios y archivos en el recurso compartido de archivos, incluido el directorio raíz. No olvide reemplazar los valores del marcador de posición en el ejemplo por los propios.

```
icacls <mounted-drive-letter>: /grant <user-email>:(f)
```

Para más información sobre cómo usar icacls para establecer los permisos de NTFS y sobre los distintos tipos de permisos admitidos, consulte [la referencia de línea de comandos de icacls](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls).

## <a name="mount-a-file-share-from-a-domain-joined-vm"></a>Montar un recurso compartido de archivos desde una máquina virtual unida al dominio

El siguiente proceso comprueba que las credenciales de Azure AD se han configurado correctamente y que puede acceder a un recurso compartido de archivos de Azure desde una máquina virtual unida a un dominio:

Inicie sesión en la máquina virtual con la identidad de Azure AD a la que se hayan concedido permisos, tal como se muestra en la siguiente imagen.

![Captura de pantalla que muestra la pantalla Inicio de sesión de Azure AD para la autenticación de usuario](media/storage-files-active-directory-enable/azure-active-directory-authentication-dialog.png)

Use el comando siguiente para montar el recurso compartido de archivos de Azure. Reemplace los valores del marcador de posición por los propios. Dado que ya se ha autenticado, no tiene que proporcionar la clave de la cuenta de almacenamiento ni el nombre de usuario y la contraseña de Azure AD. Azure AD a través de SMB admite la experiencia de inicio de sesión único con las credenciales de Azure AD.

```
net use <desired-drive-letter>: \\<storage-account-name>.file.core.windows.net\<share-name>
```

Ahora ha habilitado correctamente la autenticación de Azure AD a través de SMB y ha asignado un rol personalizado que proporciona acceso a un recurso compartido de archivos de Azure con una identidad de Azure AD. Para conceder a otros usuarios acceso al recurso compartido de archivos, siga las instrucciones de las secciones [Asignar permisos de acceso a una identidad](#assign-access-permissions-to-an-identity) y [Configurar los permisos NTFS a través de SMB](#configure-ntfs-permissions-over-smb).

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure Files y el uso de Azure AD a través de SMB, consulte estos recursos:

- [Introducción a Azure Files](storage-files-introduction.md)
- [Introducción a la autenticación de Azure Active Directory sobre SMB para Azure Files](storage-files-active-directory-overview.md)
- [P+F](storage-files-faq.md)
