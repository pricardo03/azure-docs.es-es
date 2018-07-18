---
title: Uso de bases de datos SQL en Azure Stack | Microsoft Docs
description: Aprenda a implementar las bases de datos SQL como un servicio en Azure Stack y los pasos para implementar de forma rápida el adaptador del proveedor de recursos de SQL Server.
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
ms.openlocfilehash: e1505761a0bd1ea9dabdd0b2cbab7af902198311
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938339"
---
# <a name="deploy-the-sql-server-resource-provider-on-azure-stack"></a>Implementación del proveedor de recursos de SQL Server en Azure Stack

Use el proveedor de recursos de SQL Server de Azure Stack para exponer las bases de datos SQL como un servicio de Azure Stack. El proveedor de recursos de SQL se ejecuta como un servicio en una máquina virtual (VM) Server Core de Windows Server 2016.

## <a name="prerequisites"></a>requisitos previos

Hay varios requisitos previos que se deben cumplir antes de implementar el proveedor de recursos SQL de Azure Stack. Para cumplir estos requisitos, realice los pasos siguientes en un equipo que pueda acceder a la máquina virtual de punto de conexión con privilegios:

- Si aún no lo ha hecho, [registre Azure Stack](.\azure-stack-registration.md) en Azure para poder descargar elementos de Azure Marketplace.
- Agregue la máquina virtual Windows Server Core a Marketplace de Azure Stack mediante la descarga de la imagen **Windows Server 2016 Datacenter - Server Core**. También puede usar un script para crear una [imagen de Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). Asegúrese de que selecciona la opción principal cuando se ejecuta el script.

  >[!NOTE]
  >Si tiene que instalar una actualización, puede colocar un único paquete MSU en la ruta de acceso local de la dependencia. Si hay más de un archivo MSU, se produce un error al instalar el proveedor de recursos SQL.

- Descargue el archivo binario del proveedor de recursos SQL y ejecute el extractor automático para extraer el contenido en un directorio temporal. El proveedor de recursos tiene una compilación mínima correspondiente de Azure Stack. Asegúrese de descargar el archivo binario correcto para la versión de Azure Stack que ejecuta.

    |Versión de Azure Stack|Versión de SQL RP|
    |-----|-----|
    |Versión 1804 (1.0.180513.1)|[SQL RP, versión 1.1.24.0](https://aka.ms/azurestacksqlrp1804)
    |Versión 1802 (1.0.180302.1)|[SQL RP, versión 1.1.18.0](https://aka.ms/azurestacksqlrp1802)|
    |Versión 1712 (1.0.180102.3, 1.0.180103.2 o 1.0.180106.1 [sistemas integrados])|[SQL RP, versión 1.1.14.0](https://aka.ms/azurestacksqlrp1712)|
    |     |     |

### <a name="certificates"></a>Certificados

Solo para instalaciones de sistemas integrados. Debe proporcionar el certificado PKI de PaaS de SQL que se describe en la sección de certificados de PaaS opcionales de los [requisitos de PKI de la implementación de Azure Stack](.\azure-stack-pki-certs.md#optional-paas-certificates). Coloque el archivo .pfx en la ubicación especificada por el parámetro **DependencyFilesLocalPath**.

## <a name="deploy-the-sql-resource-provider"></a>Implementar el proveedor de recursos SQL

Una vez instalados todos los requisitos previos, ejecute el script **DeploySqlProvider.ps1** para implementar el proveedor de recursos de SQL. El script DeploySqlProvider.ps1 se extrae como parte del archivo binario del proveedor de recursos de SQL descargado para su versión de Azure Stack.

> [!IMPORTANT]
> El sistema en el que se ejecuta el script debe ser Windows 10 o Windows Server 2016 con la versión más reciente instalada del entorno de ejecución .NET.

Para implementar el proveedor de recursos de SQL, abra una **nueva** ventana de consola de PowerShell con privilegios elevados y cambie al directorio en el que ha extraído los archivos binarios del proveedor de recursos de SQL. Se recomienda usar una nueva ventana de PowerShell para evitar posibles problemas ocasionados por los módulos de PowerShell que ya están cargados.

Ejecute el script DeploySqlProvider.ps1, que realiza las tareas siguientes:

- Carga los certificados y los demás artefactos en una cuenta de almacenamiento de Azure Stack.
- Publica paquetes de la galería para poder implementar las bases de datos SQL mediante la galería.
- Publica un paquete de galería para implementar los servidores de hospedaje.
- Implementa una máquina virtual mediante la imagen principal de Windows Server 2016 descargada y, luego, instala el proveedor de recursos de SQL.
- Registra un registro de DNS local que se asigna a la VM del proveedor de recursos.
- Registra el proveedor de recursos en la instancia local de Azure Resource Manager para las cuentas de usuario y operador.
- Opcionalmente, instala una única actualización de Windows Server durante la instalación del proveedor de recursos.

> [!NOTE]
> Cuando se inicia la implementación del proveedor de recursos de SQL, se crea el grupo de recursos **system.local.sqladapter**. Las cuatro implementaciones necesarias de este grupo de recursos pueden tardar hasta 75 minutos en finalizar.

### <a name="deploysqlproviderps1-parameters"></a>Parámetros de DeploySqlProvider.ps1

Puede especificar los parámetros siguientes en la línea de comandos. Si no lo hace, o se produce un error en la validación de algún parámetro, se le pedirá que proporcione los parámetros necesarios.

| Nombre de parámetro | DESCRIPCIÓN | Comentario o valor predeterminado |
| --- | --- | --- |
| **CloudAdminCredential** | Credencial del administrador de la nube, necesaria para el acceso al punto de conexión con privilegios. | _Obligatorio_ |
| **AzCredential** | Credenciales de la cuenta de administrador de servicio de Azure Stack. Use las mismas credenciales que para la implementación de Azure Stack. | _Obligatorio_ |
| **VMLocalCredential** | Credenciales para la cuenta de administrador local de la VM del proveedor de recursos de SQL. | _Obligatorio_ |
| **PrivilegedEndpoint** | Dirección IP o nombre DNS del punto de conexión con privilegios. |  _Obligatorio_ |
| **DependencyFilesLocalPath** | El archivo .pfx de certificados se debe colocar también en este directorio. | _Opcional_ (_obligatorio_ para sistemas integrados) |
| **DefaultSSLCertificatePassword** | Contraseña para el certificado .pfx. | _Obligatorio_ |
| **MaxRetryCount** | El número de veces que quiere volver a intentar cada operación si se produce un error.| 2 |
| **RetryDuration** | Intervalo de tiempo de expiración entre reintentos, en segundos. | 120 |
| **Desinstalación** | Se quita el proveedor de recursos y todos los recursos asociados (vea las notas siguientes). | Sin  |
| **DebugMode** | Impide la limpieza automática en caso de error. | Sin  |

>[!NOTE]
> Las SKU pueden tardar hasta una hora en estar visibles en el portal. No se puede crear una base de datos hasta que la SKU esté implementada y en ejecución.

## <a name="deploy-the-sql-resource-provider-using-a-custom-script"></a>Implementar el proveedor de recursos SQL con un script personalizado

Para eliminar cualquier configuración manual al implementar el proveedor de recursos, puede personalizar el script siguiente. Cambie la información de cuenta predeterminada y las contraseñas según sea necesario para la implementación de Azure Stack.

```powershell
# Install the AzureRM.Bootstrapper module and set the profile.
Install-Module -Name AzureRm.BootStrapper -Force
Use-AzureRmProfile -Profile 2017-03-09-profile

# Use the NetBIOS name for the Azure Stack domain. On the Azure Stack SDK, the default is AzureStack but could have been changed at install time.
$domain = "AzureStack"

# For integrated systems, use the IP address of one of the ERCS virtual machines
$privilegedEndpoint = "AzS-ERCS01"

# Point to the directory where the resource provider installation files were extracted.
$tempDir = 'C:\TEMP\SQLRP'

# The service admin account can be Azure Active Directory or Active Directory Federation Services.
$serviceAdmin = "admin@mydomain.onmicrosoft.com"
$AdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$AdminCreds = New-Object System.Management.Automation.PSCredential ($serviceAdmin, $AdminPass)

# Set credentials for the new resource provider VM local administrator account.
$vmLocalAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$vmLocalAdminCreds = New-Object System.Management.Automation.PSCredential ("sqlrpadmin", $vmLocalAdminPass)

# Add the cloudadmin credential that's required for privileged endpoint access.
$CloudAdminPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force
$CloudAdminCreds = New-Object System.Management.Automation.PSCredential ("$domain\cloudadmin", $CloudAdminPass)

# Change the following as appropriate.
$PfxPass = ConvertTo-SecureString "P@ssw0rd1" -AsPlainText -Force

# Change to the directory If folder where you extracted the installation files.
# Then adjust the endpoints.
. $tempDir\DeploySQLProvider.ps1 `
    -AzCredential $AdminCreds `
    -VMLocalCredential $vmLocalAdminCreds `
    -CloudAdminCredential $cloudAdminCreds `
    -PrivilegedEndpoint $privilegedEndpoint `
    -DefaultSSLCertificatePassword $PfxPass `
    -DependencyFilesLocalPath $tempDir\cert

 ```

Cuando finalice el script de instalación del proveedor de recursos, actualice el explorador para asegurarse de que puede ver las actualizaciones más recientes.

## <a name="verify-the-deployment-using-the-azure-stack-portal"></a>Comprobación de la implementación mediante Azure Stack Portal

Puede usar los pasos siguientes para verificar que el proveedor de recursos de SQL se implementa correctamente.

1. Inicie sesión en el portal de administración como administrador de servicios.
2. Seleccione **Grupos de recursos**.
3. A continuación, seleccione el grupo de recursos **system.\<location\>.sqladapter**.
4. El mensaje de **Implementaciones**, que se muestra en la siguiente captura de pantalla, debería ser **4 Succeeded** (4 Correcto).

      ![Comprobar la implementación del proveedor de recursos de SQL](./media/azure-stack-sql-rp-deploy/sqlrp-verify.png)

5. Puede obtener información más detallada sobre la implementación del proveedor de recursos en **CONFIGURACIÓN**. Seleccione **Implementaciones** para obtener información como la siguiente: ESTADO, MARCA DE TIEMPO y DURACIÓN de cada implementación.

## <a name="next-steps"></a>Pasos siguientes

[Add hosting servers](azure-stack-sql-resource-provider-hosting-servers.md) (Agregar servidores de hospedaje)
