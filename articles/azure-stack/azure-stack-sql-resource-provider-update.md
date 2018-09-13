---
title: Actualización del proveedor de recursos de SQL de Azure Stack | Microsoft Docs
description: Aprenda cómo actualizar el proveedor de recursos de SQL de Azure Stack.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 3517114d5bc267aa32cea49161d0d34156a2ed1e
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2018
ms.locfileid: "44390916"
---
# <a name="update-the-sql-resource-provider"></a>Actualización del proveedor de recursos de SQL

*Se aplica a: sistemas integrados de Azure Stack*.

Es posible que cuando Azure Stack se actualice a una nueva compilación, se lance un nuevo proveedor de recursos de SQL. Aunque el adaptador existente continúa funcionando, se recomienda actualizar a la compilación más reciente lo antes posible.

>[!IMPORTANT]
>Debe instalar las actualizaciones en el orden en que se lancen. No se puede saltar ninguna versión. Consulte la lista de versiones en [Requisitos previos para implementar el proveedor de recursos](.\azure-stack-sql-resource-provider-deploy.md#prerequisites).

## <a name="overview"></a>Información general

Para actualizar el proveedor de recursos, use el script *UpdateSQLProvider.ps1*. El script se incluye con la descarga del nuevo proveedor de recursos de SQL. El proceso de actualización es similar al proceso usado para [implementar el proveedor de recursos](.\azure-stack-sql-resource-provider-deploy.md). El script de actualización usa los mismos argumentos que el script DeploySqlProvider.ps1, y se deberá proporcionar información del certificado.

### <a name="update-script-processes"></a>Procesos de script de actualización

El script *UpdateSQLProvider.ps1* crea una nueva máquina virtual (VM) con el código del proveedor de recursos más reciente.

>[!NOTE]
>Se recomienda descargar la imagen de Windows Server 2016 Core más reciente de Marketplace Management (Administración de Marketplace). Si tiene que instalar una actualización, puede colocar un **único** paquete MSU en la ruta de acceso local de la dependencia. El script dará error si hay más de un archivo MSU en esta ubicación.

Después de que el script *UpdateSQLProvider.ps1* crea una nueva máquina virtual, migra la siguiente configuración desde la máquina virtual del proveedor antiguo:

* La información de base de datos
* La información del servidor de hospedaje
* El registro DNS necesario

### <a name="update-script-powershell-example"></a>Ejemplo de actualización del script de PowerShell

<a name="you-can-edit-and-run-the-following-script-from-an-elevated-powershell-ise"></a>Puede editar y ejecutar el script siguiente desde PowerShell ISE con privilegios elevados. 
-  
- No olvide cambiar la información de la cuenta y las contraseñas para su entorno, según corresponda.

> [!NOTE]
> Este proceso de actualización solo se aplica a sistemas integrados de Azure Stack.

```powershell
# Install the AzureRM.Bootstrapper module and set the profile.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but this might have been changed at installation.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines.
$privilegedEndpoint = "AzS-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service administrator account (this can be Azure AD or AD FS).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the new resource provider VM.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# Add the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change directory to the folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\UpdateSQLProvider.ps1 -AzCredential $AdminCreds `
  -VMLocalCredential $vmLocalAdminCreds `
  -CloudAdminCredential $cloudAdminCreds `
  -PrivilegedEndpoint $privilegedEndpoint `
  -DefaultSSLCertificatePassword $PfxPass `
  -DependencyFilesLocalPath $tempDir\cert `

 ```

## <a name="updatesqlproviderps1-parameters"></a>Parámetros de UpdateSQLProvider.ps1

Puede especificar los siguientes parámetros desde la línea de comandos al ejecutar el script. Si no lo hace, o se produce un error en la validación de algún parámetro, se le pedirá que proporcione los parámetros necesarios.

| Nombre de parámetro | DESCRIPCIÓN | Comentario o valor predeterminado |
| --- | --- | --- |
| **CloudAdminCredential** | Credencial del administrador de la nube, necesaria para el acceso al punto de conexión con privilegios. | _Obligatorio_ |
| **AzCredential** | Las credenciales de la cuenta de administrador del servicio Azure Stack. Use las mismas credenciales que para la implementación de Azure Stack. | _Obligatorio_ |
| **VMLocalCredential** | Credenciales para la cuenta de administrador local de la VM del proveedor de recursos de SQL. | _Obligatorio_ |
| **PrivilegedEndpoint** | Dirección IP o nombre DNS del punto de conexión con privilegios. |  _Obligatorio_ |
| **AzureEnvironment** | El entorno de Azure de la cuenta de administrador de servicio que ha usado para la implementación de Azure Stack. Necesario únicamente si no es un sistema de archivos distribuido de Azure. Los nombres de entorno que se admiten son **AzureCloud**, **AzureUSGovernment** o, si usa una suscripción a Azure Active Directory de China, **AzureChinaCloud**. | AzureCloud |
| **DependencyFilesLocalPath** | También debe incluir el archivo .pfx del certificado en este directorio. | _Opcional para un único nodo, pero obligatorio para varios nodos_ |
| **DefaultSSLCertificatePassword** | Contraseña para el certificado .pfx. | _Obligatorio_ |
| **MaxRetryCount** | El número de veces que quiere volver a intentar cada operación si se produce un error.| 2 |
| **RetryDuration** |Intervalo de tiempo de expiración entre reintentos, en segundos. | 120 |
| **Desinstalación** | Quita el proveedor de recursos y todos los recursos asociados. | Sin  |
| **DebugMode** | Impide la limpieza automática en caso de error. | Sin  |

## <a name="next-steps"></a>Pasos siguientes

[Mantenimiento del proveedor de recursos SQL](azure-stack-sql-resource-provider-maintain.md)
