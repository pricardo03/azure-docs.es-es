---
title: Habilitación de la autenticación de Azure Active Directory en SMB para Azure Files
description: Aprenda a habilitar la autenticación basada en la identidad en SMB para recursos compartidos de archivos de Azure mediante Active Directory. Las máquinas virtuales Windows unidas a un dominio pueden acceder entonces a los recursos compartidos de archivos de Azure con las credenciales de AD.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 02/23/2020
ms.author: rogarana
ms.openlocfilehash: 9bda3331bc27f387bd05ea0fcd29e7badf6d6a02
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2020
ms.locfileid: "77651090"
---
# <a name="enable-active-directory-authentication-over-smb-for-azure-file-shares"></a>Habilitación de la autenticación de Azure Active Directory en SMB para recursos compartidos de archivos de Azure

[Azure Files](storage-files-introduction.md) admite la autenticación basada en la identidad en el bloque de mensajes del servidor (SMB) mediante dos tipos de servicios de dominio: Azure Active Directory Domain Services (Azure AD DS) (GA) y Active Directory (AD) (versión preliminar). Este artículo se centra en la posibilidad recién introducida (versión preliminar) de aprovechar Active Directory Domain Services para la autenticación en recursos compartidos de archivos de Azure. Si está interesado en habilitar la autenticación de Azure AD DS (GA) en recursos compartidos de archivos de Azure, consulte [nuestro artículo sobre el tema](storage-files-identity-auth-active-directory-domain-service-enable.md).

> [!NOTE]
> Los recursos compartidos de archivos de Azure solo admiten la autenticación en un servicio de dominio: Azure Active Directory Domain Services (Azure AD DS) o Active Directory (AD). 
>
> Las identidades de AD usadas para la autenticación de recursos compartidos de archivos de Azure se deben sincronizar con Azure AD. La sincronización de hash de contraseña es opcional. 
> 
> La autenticación de AD no admite la autenticación en cuentas de equipo creadas en AD. 
> 
> Solo se puede admitir la autenticación de AD en un bosque de AD en el que se haya registrado la cuenta de almacenamiento. De forma predeterminada, solo puede acceder a recursos compartidos de archivos de Azure con las credenciales de AD desde un solo bosque de AD. Si necesita acceso al recurso compartido de archivos de Azure desde otro bosque, asegúrese de tener configurada la confianza de bosque adecuada; para más información, consulte las [preguntas más frecuentes](https://docs.microsoft.com/azure/storage/files/storage-files-faq#security-authentication-and-access-control).  
> 
> Se admite la autenticación de AD para el acceso SMB y la persistencia de ACL en los recursos compartidos de archivos de Azure administrados por Azure File Sync.
>
> Azure Files admite la autenticación Kerberos con AD con cifrado RC4-HMAC. Todavía no se admite el cifrado de Kerberos con AES.

Al habilitar AD para recursos compartidos de archivos de Azure en SMB, las máquinas unidas a un dominio de AD pueden montar recursos compartidos de archivos de Azure con las credenciales de AD existentes. Esta funcionalidad se puede habilitar con un entorno de AD hospedado en máquinas locales o en Azure.

Las identidades de AD que se usan para acceder a recursos compartidos de archivos de Azure se deben sincronizar con Azure AD para aplicar los permisos de archivo de nivel de recurso compartido mediante el modelo estándar de [control de acceso basado en rol (RBAC)](../../role-based-access-control/overview.md). Se conservarán y aplicarán las [DACL tipo Windows](https://docs.microsoft.com/previous-versions/technet-magazine/cc161041(v=msdn.10)?redirectedfrom=MSDN) en archivos o directorios transferidos desde servidores de archivos existentes. Esta característica ofrece una perfecta integración con la infraestructura de dominio de AD de la empresa. A medida que reemplaza los servidores de archivos locales por recursos compartidos de archivos de Azure, los usuarios existentes pueden acceder a estos desde sus clientes actuales con una experiencia de inicio de sesión único, sin ningún cambio en las credenciales en uso.  
 
## <a name="prerequisites"></a>Prerrequisitos 

Antes de habilitar la autenticación de AD para los recursos compartidos de archivos de Azure, asegúrese de que cumple los requisitos previos siguientes: 

- Seleccionar o crear el entorno de AD y sincronizarlo con Azure AD. 

    Puede habilitar la característica en un entorno de AD nuevo o existente. Las identidades usadas para el acceso deben estar sincronizadas con Azure AD. El inquilino de Azure AD y el recurso compartido de archivos al que accede debe estar asociado con la misma suscripción. 

    Para configurar un entorno de dominio de AD, consulte [Introducción a Active Directory Domain Services](https://docs.microsoft.com/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview). Si no ha sincronizado su instancia de AD con Azure AD, siga las instrucciones que se indican en [¿Qué es la identidad híbrida con Azure Active Directory?](../../active-directory/hybrid/whatis-hybrid-identity.md) con el fin de determinar el método de autenticación preferido y la opción de configuración de Azure AD Connect. 

- Unir una máquina local o una máquina virtual de Azure a un dominio de AD (lo que también se conoce como AD DS). 

    Para acceder a un recurso compartido de archivos mediante las credenciales de AD desde una máquina local o virtual, el dispositivo debe estar unido a un dominio de AD. Para información acerca de cómo unirse a un dominio de AD, consulte [Unir un equipo a un dominio](https://docs.microsoft.com/windows-server/identity/ad-fs/deployment/join-a-computer-to-a-domain). 

- Seleccionar o crear una cuenta de almacenamiento de Azure en [una región admitida](#regional-availability). 

    Asegúrese de que la cuenta de almacenamiento que contiene los recursos compartidos de archivos todavía no está configurada para la autenticación de Azure AD DS. Si la autenticación de Azure AD DS de Azure Files está habilitada en la cuenta de almacenamiento, se debe deshabilitar antes de cambiar al uso de AD. Esto implica que las ACL existentes configuradas en el entorno de Azure AD DS se deberán volver a configurar para que se apliquen los permisos adecuados.
    
    Para información sobre cómo crear un nuevo recurso compartido de archivos, consulte  [Creación de un recurso compartido de archivos en Azure Files](storage-how-to-create-file-share.md).
    
    Para conseguir un rendimiento óptimo, se recomienda implementar la cuenta de almacenamiento en la misma región que la máquina virtual desde la que vaya a acceder al recurso compartido. 

- Para comprobar la conectividad, monte los recursos compartidos de archivos de Azure con la clave de su cuenta de almacenamiento. 

    Para comprobar que el dispositivos y el recurso compartido de archivos están configurados correctamente, intente montar el recurso compartido de archivos con la clave de la cuenta de almacenamiento. Para más información, consulte  [Uso de un recurso compartido de archivos de Azure con Windows](storage-how-to-use-files-windows.md).

## <a name="regional-availability"></a>Disponibilidad regional

La autenticación de Azure Files está disponible en la [mayoría de las regiones públicas](https://azure.microsoft.com/global-infrastructure/regions/); el subconjunto de regiones en las que todavía no está disponible es:

- Oeste de EE. UU.
- Oeste de EE. UU. 2
- Este de EE. UU.
- Este de EE. UU. 2
- Centro-sur de EE. UU.
- Oeste de Europa
- Norte de Europa

## <a name="workflow-overview"></a>Introducción al flujo de trabajo

Antes de habilitar la autenticación de AD en SMB para recursos compartidos de archivos de Azure, le recomendamos que revise los  [requisitos previos](#prerequisites)  y que se asegure de haber completado todos los pasos. Los requisitos previos validan que los entornos de AD, Azure AD y Azure Storage estén configurados correctamente. 

A continuación, conceda acceso a los recursos de Azure Files con las credenciales de AD: 

- Habilite la autenticación de AD de Azure Files en la cuenta de almacenamiento.  

- Asigne permisos de acceso para un recurso compartido a la identidad de Azure AD (un usuario, grupo o entidad de servicio) que esté sincronizada con la identidad de AD de destino. 

- Configure las ACL en SMB para directorios y archivos. 

- Monte un recurso compartido de archivos de Azure desde una máquina virtual unida al dominio de AD. 

En el diagrama siguiente se ilustra el flujo de trabajo de un extremo a otro para habilitar la autenticación de Azure AD en SMB en los recursos compartidos de archivos de Azure. 

![Diagrama de flujo de trabajo de AD de Files](media/storage-files-active-directory-domain-services-enable/diagram-files-ad.png)

> [!NOTE]
> La autenticación de AD en SMB para los recursos compartidos de archivos de Azure solo se admite en máquinas locales o virtuales que se ejecutan en versiones de sistema operativo posteriores a Windows 7 o Windows Server 2008 R2. 

## <a name="enable-ad-authentication-for-your-account"></a>Habilitación de la autenticación de AD para su cuenta 

Para habilitar la autenticación de AD en SMB para los recursos compartidos de archivos de Azure, primero debe registrar la cuenta de almacenamiento en AD y, luego, establecer las propiedades de dominio necesarias en la cuenta de almacenamiento. Cuando la característica está habilitada en la cuenta de almacenamiento, se aplica a todos los recursos compartidos de archivos nuevos y existentes de la cuenta. Use `join-AzStorageAccountForAuth` para habilitar la característica. Puede encontrar la descripción detallada del flujo de trabajo de un extremo a otro en la sección siguiente. 

> [!IMPORTANT]
> El cmdlet `join-AzStorageAccountForAuth` realizará modificaciones en el entorno de AD. Lea la explicación siguiente para comprender mejor lo que está haciendo para asegurarse de que tiene los permisos adecuados para ejecutar el comando y que los cambios aplicados se alinean con las directivas de cumplimiento y seguridad. 

El cmdlet `join-AzStorageAccountForAuth` realizará la acción equivalente a la unión a un dominio sin conexión en nombre de la cuenta de almacenamiento indicada. Se creará una cuenta en el dominio de AD, bien una [cuenta de equipo](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-accounts#manage-default-local-accounts-in-active-directory) o una [cuenta de inicio de sesión de servicio](https://docs.microsoft.com/windows/win32/ad/about-service-logon-accounts). La cuenta de AD creada representa la cuenta de almacenamiento en el dominio de AD. Si la cuenta de AD se crea en una unidad organizativa (OU) de AD que aplica la expiración de contraseñas, debe actualizar la contraseña antes de su vigencia máxima. Si no actualiza la contraseña de la cuenta de AD, se producirán errores de autenticación al acceder a recursos compartidos de archivos de Azure. Para saber cómo actualizar la contraseña, consulte [Actualización de la contraseña de la cuenta de AD](#update-ad-account-password).

Puede usar el script siguiente para realizar el registro y habilitar la característica o, como alternativa, puede realizar manualmente las operaciones que el script haría. Dichas operaciones se describen en la sección que sigue al script. No es necesario hacer ambas cosas.

### <a name="1-check-prerequisites"></a>1. Comprobación de los requisitos previos
- [Descarga y descompresión del módulo AzFilesHybrid](https://github.com/Azure-Samples/azure-files-samples/releases)
- Instale y ejecute el módulo en un dispositivo que se haya unido a un dominio de AD con credenciales de AD que tengan permisos para crear una cuenta de inicio de sesión de servicio o una cuenta de equipo en la instancia de destino.
-  Ejecute el script con una credencial de AD que esté sincronizada con su instancia de Azure AD. La credencial de AD debe tener los permisos de la cuenta de propietario o del rol de RBAC de colaborador.
- Asegúrese de que la cuenta de almacenamiento se encuentra en una [región admitida](#regional-availability).

### <a name="2-execute-ad-enablement-script"></a>2. Ejecución del script de habilitación de AD
No olvide reemplazar los valores de marcador de posición por los suyos propios en los parámetros siguientes antes de ejecutar el script en PowerShell.

```PowerShell
#Change the execution policy to unblock importing AzFilesHybrid.psm1 module
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope Currentuser

# Navigate to where AzFilesHybrid is unzipped and stored and run to copy the files into your path
.\CopyToPSPath.ps1 

#Import AzFilesHybrid module
Import-Module -name AzFilesHybrid

#Login with an Azure AD credential that has either storage account owner or contributer RBAC assignment
Connect-AzAccount

#Select the target subscription for the current session
Select-AzSubscription -SubscriptionId "<your-subscription-id-here>"

#Register the target storage account with your active directory environment under the target OU
join-AzStorageAccountForAuth -ResourceGroupName "<resource-group-name-here>" -Name "<storage-account-name-here>" -DomainAccountType "<ServiceLogonAccount|ComputerAccount>" -OrganizationUnitName "<ou-name-here>"
```

La descripción siguiente resume todas las acciones que se realizan cuando se ejecuta el cmdlet `join-AzStorageAccountForAuth`. Si prefiere no usar el comando, puede realizar estos pasos manualmente:

> [!NOTE]
> Si ya ha ejecutado correctamente el script join-AzStorageAccountForAuth anterior, vaya a la sección siguiente "3. Confirmación de que la característica está habilitada". No es necesario volver a realizar las operaciones siguientes.

#### <a name="a-checking-environment"></a>a. Comprobación del entorno

En primer lugar, se comprueba el entorno. En concreto, se comprueba si [Active Directory PowerShell](https://docs.microsoft.com/powershell/module/addsadministration/?view=win10-ps) está instalado y si el shell se ejecuta con privilegios de administrador. Luego, se comprueba si está instalado el [módulo Az.Storage 1.11.1-preview](https://www.powershellgallery.com/packages/Az.Storage/1.11.1-preview) y, en caso negativo, se instala. Si los resultados de esas comprobaciones son correctos, se comprobará la instancia de AD para ver si hay una [cuenta de equipo](https://docs.microsoft.com/windows/security/identity-protection/access-control/active-directory-accounts#manage-default-local-accounts-in-active-directory) (valor predeterminado) o una [cuenta de inicio de sesión de servicio](https://docs.microsoft.com/windows/win32/ad/about-service-logon-accounts) que ya se haya creado con SPN/UPN como "cifs/your-storage-account-name-here.file.core.windows.net". Si la cuenta no existe, se creará una como se describe en la sección b que aparece a continuación.

#### <a name="b-creating-an-identity-representing-the-storage-account-in-your-ad-manually"></a>b. Creación de una identidad que representa la cuenta de almacenamiento en AD manualmente

Para crear esta cuenta manualmente, cree una clave de Kerberos para la cuenta de almacenamiento mediante `New-AzStorageAccountKey -KeyName kerb1`. A continuación, use esa clave de Kerberos como contraseña para la cuenta. Esta clave solo se usa durante la configuración y no se puede usar para las operaciones de control o de plano de datos en la cuenta de almacenamiento.

Cuando tenga esa clave, cree una cuenta de servicio o de equipo en la unidad organizativa. Use la siguiente especificación: SPN: "cifs/your-storage-account-name-here.file.core.windows.net" Contraseña: clave Kerberos de la cuenta de almacenamiento.

Si la unidad organizativa impone la expiración de la contraseña, debe actualizar la contraseña antes de su vigencia máxima para evitar errores de autenticación al acceder a recursos compartidos de archivos de Azure. Consulte [Actualización de la contraseña de la cuenta de AD](#update-ad-account-password) para más información.

Mantenga el SID de la cuenta recién creada; lo necesitará en el siguiente paso.

##### <a name="c-enable-the-feature-on-your-storage-account"></a>c. Habilitación de la característica en la cuenta de almacenamiento

Luego, el script habilitaría la característica en la cuenta de almacenamiento. Para realizar esta configuración manualmente, proporcione algunos detalles de configuración para las propiedades de dominio en el siguiente comando y, luego, ejecútelo. El SID de la cuenta de almacenamiento necesario en el siguiente comando es el SID de la identidad que creó en AD (sección b anterior).

```PowerShell
#Set the feature flag on the target storage account and provide the required AD domain information

Set-AzStorageAccount -ResourceGroupName "<your-resource-group-name-here>" -Name "<your-storage-account-name-here>" -EnableActiveDirectoryDomainServiesForFile $true -ActiveDirectoryDomainName "<your-domain-name-here>" -ActiveDirectoryNetBiosDomainName "<your-netbios-domain-name-here>" -ActiveDirectoryForestName "<your-forest-name-here>" -ActiveDirectoryDomainGuid "<your-guid-here>" -ActiveDirectoryDomainsid "<your-domain-sid-here>" -ActiveDirectoryAzureStorageSid "<your-storage-account-sid>"
```


### <a name="3-confirm-that-the-feature-is-enabled"></a>3. Confirmación de que la característica está habilitada

Para confirmar que la característica está habilitada en la cuenta de almacenamiento, puede usar el siguiente script:

```PowerShell
#Get the target storage account
$storageaccount = Get-AzStorageAccount -ResourceGroupName "<your-resource-group-name-here>" -Name "<your-storage-account-name-here>"

#List the directory service of the selected service account
$storageAccount.AzureFilesIdentityBasedAuth.DirectoryServiceOptions

#List the directory domain information if the storage account has enabled AD authentication for file shares
$storageAccount.AzureFilesIdentityBasedAuth.ActiveDirectoryProperties
```

Ahora ha habilitado correctamente la característica en la cuenta de almacenamiento. Aunque la característica está habilitada, sigue siendo necesario realizar acciones adicionales para poder usarla correctamente.

[!INCLUDE [storage-files-aad-permissions-and-mounting](../../../includes/storage-files-aad-permissions-and-mounting.md)]

Ahora ha habilitado correctamente la autenticación de AD en SMB y ha asignado un rol personalizado que proporciona acceso a un recurso compartido de archivos de Azure con una identidad de AD. Para conceder a otros usuarios acceso al recurso compartido de archivos, siga las instrucciones de las secciones [Asignación de permisos de acceso](#assign-access-permissions-to-an-identity) y [Configuración de los permisos NTFS en SMB](#configure-ntfs-permissions-over-smb).

## <a name="update-ad-account-password"></a>Actualización de la contraseña de la cuenta de AD

Si registró la cuenta o identidad de AD que representa la cuenta de almacenamiento bajo una unidad organizativa que aplica el tiempo de expiración de la contraseña, debe rotar la contraseña antes de su vigencia máxima. Si no se actualiza la contraseña de la cuenta de AD, se producirán errores de autenticación en el acceso a recursos compartidos de archivos de Azure.  

Para desencadenar la rotación de contraseñas, puede ejecutar el comando `Update-AzStorageAccountADObjectPassword` desde el módulo AzFilesHybrid. El cmdlet realiza acciones similares a la rotación de claves de la cuenta de almacenamiento. Obtiene la segunda clave de Kerberos de la cuenta de almacenamiento y la usa para actualizar la contraseña de la cuenta registrada en AD. Después, vuelve a generar la clave de Kerberos de destino de la cuenta de almacenamiento y actualiza la contraseña de la cuenta registrada en AD. Este cmdlet se debe ejecutar en un entorno unido a un dominio de AD.

```PowerShell
#Update the password of the AD account registered for the storage account
Update-AzStorageAccountADObjectPassword -RotateToKerbKey kerb2 -ResourceGroupName "your-resource-group-name-here" -StorageAccountName "your-storage-account-name-here"
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Azure Files y el uso de AD en SMB, consulte estos recursos:

- [Introducción a la compatibilidad de la autenticación basada en la identidad de Azure Files con el acceso SMB](storage-files-active-directory-overview.md)
- [P+F](storage-files-faq.md)
