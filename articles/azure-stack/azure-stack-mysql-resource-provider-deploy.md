---
title: Uso de bases de datos MySQL en Azure Stack | Microsoft Docs
description: Aprenda a implementar bases de datos MySQL como servicio en Azure Stack y los pasos para implementar de forma rápida el adaptador del proveedor de recursos de MySQL Server.
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
ms.date: 06/25/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: e4c3eb1d7dfd4894576d5fbed52cf4de5fed9e44
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938124"
---
# <a name="deploy-the-mysql-resource-provider-on-azure-stack"></a>Implementación del proveedor de recursos MySQL en Azure Stack

Use el proveedor de recursos MySQL Server de Azure Stack para exponer las bases de datos MySQL como un servicio de Azure Stack. El proveedor de recursos MySQL se ejecuta como un servicio en una máquina virtual (VM) Server Core de Windows Server 2016.

## <a name="prerequisites"></a>requisitos previos

Hay varios requisitos previos que se deben cumplir antes de implementar el proveedor de recursos MySQL en Azure Stack. Para cumplir estos requisitos, realice los pasos de este artículo en un equipo que pueda acceder a la máquina virtual de punto de conexión con privilegios.

* Si aún no lo ha hecho, [registre Azure Stack](.\azure-stack-registration.md) en Azure para poder descargar elementos de Azure Marketplace.
* Agregue la máquina virtual principal de Windows Server a Marketplace de Azure Stack mediante la descarga de la imagen **Windows Server 2016 Datacenter - Server Core**. También puede usar un script para crear una [imagen de Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). Asegúrese de que selecciona la opción principal cuando se ejecuta el script.

  >[!NOTE]
  >Si tiene que instalar una actualización, puede colocar un único paquete .MSU en la ruta de acceso local de la dependencia. Si hay más de un archivo .MSU, se produce un error al instalar el proveedor de recursos MySQL.

* Descargue el archivo binario del proveedor de recursos MySQL y ejecute el extractor automático para extraer el contenido en un directorio temporal.

  >[!NOTE]
  >Para implementar el proveedor MySQL en un sistema sin acceso a Internet, copie el archivo [mysql-connector-net-6.10.5.msi](https://dev.mysql.com/get/Download/sConnector-Net/mysql-connector-net-6.10.5.msi) en un recurso compartido local. Proporcione el nombre del recurso compartido cuando se le pida. Debe instalar los módulos PowerShell de Azure y Azure Stack.

* El proveedor de recursos tiene una compilación mínima correspondiente de Azure Stack. Asegúrese de descargar el archivo binario correcto para la versión de Azure Stack que ejecuta.

    | Versión de Azure Stack | MySQL RP, versión|
    | --- | --- |
    | Versión 1804 (1.0.180513.1)|[MySQL RP, versión 1.1.24.0](https://aka.ms/azurestackmysqlrp1804) |
    | Versión 1802 (1.0.180302.1) | [MySQL RP, versión 1.1.18.0](https://aka.ms/azurestackmysqlrp1802) |
    | Versión 1712 (1.0.180102.3 o 1.0.180106.1 [sistemas integrados]) | [MySQL RP, versión 1.1.14.0](https://aka.ms/azurestackmysqlrp1712) |

### <a name="certificates"></a>Certificados

Para el ASDK, se crea un certificado autofirmado como parte del proceso de instalación. Para un sistema integrado de Azure Stack, debe proporcionar un certificado adecuado. Si tiene que proporcionar su propio certificado, coloque un archivo .pfx en **DependencyFilesLocalPath** que cumpla los criterios siguientes:

* Un certificado comodín para \*.dbadapter.\<región\>.\<FQDN externo\> o un certificado de entidad de certificación único con un nombre común de mysqladapter.dbadapter.\<región\>.\<FQDN externo\>.
* Este certificado debe ser de confianza. La cadena de confianza debe existir sin necesidad de certificados intermedios.
* En DependencyFilesLocalPath solo existe un archivo de certificado individual.
* El nombre de archivo no puede contener espacios ni caracteres especiales.

## <a name="deploy-the-resource-provider"></a>Implementar el proveedor de recursos

Una vez instalados todos los requisitos previos, ejecute el script **DeployMySqlProvider.ps1** para implementar el proveedor de recursos MYSQL. El script DeployMySqlProvider.ps1 se extrae como parte del archivo binario del proveedor de recursos MySQL descargado para su versión de Azure Stack.

> [!IMPORTANT]
> El sistema en el que se ejecuta el script debe ser Windows 10 o Windows Server 2016 con la versión más reciente instalada del entorno de ejecución .NET.

Para implementar el proveedor de recursos MySQL, abra una nueva ventana de consola de PowerShell con privilegios elevados y cambie al directorio en el que ha extraído los archivos binarios del proveedor de recursos MySQL. Se recomienda usar una nueva ventana de PowerShell para evitar posibles problemas ocasionados por los módulos de PowerShell que ya están cargados.

Ejecute el script **DeployMySqlProvider.ps1**, que realiza las tareas siguientes:

* Carga los certificados y los demás artefactos en una cuenta de almacenamiento de Azure Stack.
* Publica paquetes de la galería para poder implementar las bases de datos MySQL mediante esta.
* Publica un paquete de galería para implementar los servidores de hospedaje.
* Implementa una máquina virtual mediante la imagen principal de Windows Server 2016 descargada y, luego, instala el proveedor de recursos MySQL.
* Registra un registro de DNS local que se asigna a la VM del proveedor de recursos.
* Registra el proveedor de recursos en la instancia local de Azure Resource Manager para las cuentas de usuario y operador.
* Opcionalmente, instala una única actualización de Windows Server durante la instalación del proveedor de recursos.

> [!NOTE]
> Cuando se inicia la implementación del proveedor de recursos MySQL, se crea el grupo de recursos **system.local.mysqladapter**. Las cuatro implementaciones necesarias de este grupo de recursos pueden tardar hasta 75 minutos en finalizar.

### <a name="deploymysqlproviderps1-parameters"></a>Parámetros de DeployMySqlProvider.ps1

Puede especificar estos parámetros en la línea de comandos. Si no lo hace, o se produce un error en la validación de algún parámetro, se le pedirá que proporcione los parámetros necesarios.

| Nombre de parámetro | DESCRIPCIÓN | Comentario o valor predeterminado |
| --- | --- | --- |
| **CloudAdminCredential** | Credencial del administrador de la nube, necesaria para el acceso al punto de conexión con privilegios. | _Obligatorio_ |
| **AzCredential** | Credenciales de la cuenta de administrador de servicio de Azure Stack. Use las mismas credenciales que para la implementación de Azure Stack. | _Obligatorio_ |
| **VMLocalCredential** | Las credenciales para la cuenta de administrador local de la VM del proveedor de recursos de MySQL. | _Obligatorio_ |
| **PrivilegedEndpoint** | Dirección IP o nombre DNS del punto de conexión con privilegios. |  _Obligatorio_ |
| **DependencyFilesLocalPath** | Ruta de acceso a un recurso compartido local que contiene [mysql-connector-net-6.10.5.msi](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.10.5.msi). Si se proporciona una de estas rutas de acceso, el archivo de certificados debe colocarse también en este directorio. | _Opcional_ para un único nodo, pero _obligatorio_ para varios nodos. |
| **DefaultSSLCertificatePassword** | Contraseña para el certificado .pfx. | _Obligatorio_ |
| **MaxRetryCount** | Número de veces que quiere volver a intentar cada operación si se produce un error.| 2 |
| **RetryDuration** | Intervalo de tiempo de expiración entre reintentos, en segundos. | 120 |
| **Desinstalación** | Se quita el proveedor de recursos y todos los recursos asociados (vea las notas siguientes). | Sin  |
| **DebugMode** | Impide la limpieza automática en caso de error. | Sin  |
| **AcceptLicense** | Omite el aviso para aceptar la licencia GPL.  <http://www.gnu.org/licenses/old-licenses/gpl-2.0.html> | |

> [!NOTE]
> Las SKU pueden tardar hasta una hora en estar visibles en el portal. No se puede crear una base de datos hasta que la SKU esté implementada y en ejecución.

## <a name="deploy-the-mysql-resource-provider-using-a-custom-script"></a>Implementación del proveedor de recursos MySQL con un script personalizado

Para eliminar cualquier configuración manual al implementar el proveedor de recursos, puede personalizar el script siguiente. Cambie la información de cuenta predeterminada y las contraseñas según sea necesario para su implementación de Azure Stack.

```powershell
# Install the AzureRM.Bootstrapper module and set the profile.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time.
$domain = "AzureStack"  

# For integrated systems, use the IP address of one of the ERCS virtual machines
$privilegedEndpoint = "AzS-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\MYSQLRP'

# The service admin account (can be Azure Active Directory or Active Directory Federation Services).
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set the credentials for the new resource provider VM local administrator account
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("mysqlrpadmin", $vmLocalAdminPass)

# And the cloudadmin credential required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Run the installation script from the folder where you extracted the installation files.
# Find the ERCS01 IP address first, and make sure the certificate file is in the specified directory.
. $tempDir\DeployMySQLProvider.ps1 `
    -AzCredential $AdminCreds `
    -VMLocalCredential $vmLocalAdminCreds `
    -CloudAdminCredential $cloudAdminCreds `
    -PrivilegedEndpoint $privilegedEndpoint `
    -DefaultSSLCertificatePassword $PfxPass `
    -DependencyFilesLocalPath $tempDir\cert `
    -AcceptLicense

```

Cuando finalice el script de instalación del proveedor de recursos, actualice el explorador para asegurarse de que puede ver las actualizaciones más recientes.

## <a name="verify-the-deployment-by-using-the-azure-stack-portal"></a>Comprobación de la implementación mediante Azure Stack Portal

1. Inicie sesión en el portal de administración como administrador de servicios.
2. Seleccione **Resource Groups** (Grupos de recursos).
3. Seleccione el grupo de recursos **system.\<ubicación\>.mysqladapter**.
4. En la página de resumen de la información general del grupo de recursos, el mensaje en **Deployments** (Implementaciones) debe ser **3 Succeeded** (3 Correcto).
5. Puede obtener información más detallada sobre la implementación del proveedor de recursos en **SETTINGS** (Configuración). Seleccione **Deployments** (Implementaciones) para obtener información como: estado, marca de tiempo y duración de cada implementación.

## <a name="next-steps"></a>Pasos siguientes

[Add hosting servers](azure-stack-mysql-resource-provider-hosting-servers.md) (Agregar servidores de hospedaje)
