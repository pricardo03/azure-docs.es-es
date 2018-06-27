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
ms.date: 06/20/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 061d9431890622dafafa8c180027b8df735c100e
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296015"
---
# <a name="use-mysql-databases-on-microsoft-azure-stack"></a>Usar bases de datos MySQL en Microsoft Azure Stack
Use el proveedor de recursos de MySQL Server de Azure Stack para exponer las bases de datos MySQL como servicio de Azure Stack. El servicio del proveedor de recursos de MySQL se ejecuta en la máquina virtual del proveedor de recursos de MySQL, que es una máquina virtual de núcleo de Windows Server.

## <a name="deploy-the-resource-provider"></a>Implementar el proveedor de recursos 
1. Si no lo ha hecho ya, registre el kit de desarrollo y descargue la imagen de Windows Server 2016 Datacenter Core desde Marketplace Management (Administración de Marketplace). Debe utilizar una imagen de Windows Server 2016 Core. También puede usar un script para crear una [imagen de Windows Server 2016](https://docs.microsoft.com/azure/azure-stack/azure-stack-add-default-image). (Asegúrese de seleccionar la opción Core). 

2. Inicie sesión en un host que pueda acceder a la máquina virtual del punto de conexión con privilegios:
    - En las instalaciones del SDK de Azure Stack, inicie sesión en el host físico.  
    - En los sistemas integrados, el host debe ser un sistema que pueda acceder al punto de conexión con privilegios. 
    
    >[!NOTE] 
    > El script se *debe* ejecutar en un sistema con Windows 10 o Windows Server 2016 con la versión más reciente del runtime de .NET instalada. De lo contrario, se produce un error de instalación. El host del Kit de desarrollo de Azure Stack cumple este criterio. 
    
3. Descargue el binario del proveedor de recursos de MySQL. A continuación, ejecute el autoextractor para extraer el contenido en un directorio temporal. 
    >[!NOTE]  
    > El proveedor de recursos tiene una compilación mínima correspondiente de Azure Stack. Asegúrese de descargar el binario correcto para la versión de Azure Stack que se ejecuta.

    | Versión de Azure Stack | MySQL RP, versión| 
    | --- | --- | 
    | Versión 1804 (1.0.180513.1)|[MySQL RP, versión 1.1.24.0](https://aka.ms/azurestackmysqlrp1804) | 
    | Versión 1802 (1.0.180302.1) | [MySQL RP, versión 1.1.18.0](https://aka.ms/azurestackmysqlrp1802) | 
    | Versión 1712 (1.0.180102.3 o 1.0.180106.1 [sistemas integrados]) | [MySQL RP, versión 1.1.14.0](https://aka.ms/azurestackmysqlrp1712) | 

4.  Para el Kit de desarrollo de Azure Stack se crea un certificado autofirmado como parte de este proceso. En sistemas integrados, debe proporcionar un certificado adecuado. Si tiene que proporcionar su propio certificado, coloque un archivo .pfx en **DependencyFilesLocalPath** que cumpla los criterios siguientes: 
    - Un certificado comodín para \*.dbadapter.\<región\>.\<FQDN externo\> o un certificado de entidad de certificación único con un nombre común de mysqladapter.dbadapter.\<región\>.\<FQDN externo\>. 
    - Este certificado debe ser de confianza. Es decir, la cadena de confianza debe existir sin necesidad de certificados intermedios. 
    - En DependencyFilesLocalPath solo existe un archivo de certificado individual. 
    - El nombre de archivo no debe contener caracteres especiales o espacios. 

5. Abra una **nueva** consola de PowerShell con privilegios elevados (administrativos). A continuación, cambie al directorio desde el que extrajo los archivos. Use una nueva ventana para evitar problemas que puedan surgir de los módulos de PowerShell incorrectos que ya están cargados en el sistema. 

6. Ejecute el script **DeployMySqlProvider.ps1**. El script sigue estos pasos: 
    - Descarga el binario del conector de MySQL (puede proporcionarse sin conexión). 
    - Carga los certificados y los demás artefactos en una cuenta de almacenamiento de Azure Stack. 
    - Publica los paquetes de la galería para poder implementar las bases de datos SQL a través de esta. 
    - Publica un paquete de galería para implementar los servidores de hospedaje. 
    - Implementa una máquina virtual mediante una imagen de Marketplace de Azure Stack con Windows Server 2016 e instala al proveedor de recursos. 
    - Registra un registro de DNS local que se asigna a la VM del proveedor de recursos. 
    - Registra el proveedor de recursos en la instancia local de Azure Resource Manager (inquilino y administrador). 

    Puede especificar los parámetros necesarios en la línea de comandos o ejecutar el script sin parámetros y, después, agregarlos cuando se le solicite. 

    Este es un ejemplo que se puede ejecutar desde el símbolo del sistema de PowerShell. Asegúrese de cambiar la información de la cuenta y las contraseñas según sea necesario: 

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
    # Find the ERCS01 IP address first, and make sure the certificate 
    # file is in the specified directory. 
    $tempDir\DeployMySQLProvider.ps1 -AzCredential $AdminCreds ` 
        -VMLocalCredential $vmLocalAdminCreds ` 
        -CloudAdminCredential $cloudAdminCreds ` 
        -PrivilegedEndpoint $privilegedEndpoint ` 
        -DefaultSSLCertificatePassword $PfxPass ` 
        -DependencyFilesLocalPath $tempDir\cert ` 
        -AcceptLicense 
    ``` 

### <a name="deploymysqlproviderps1-parameters"></a>Parámetros de DeployMySqlProvider.ps1 
Puede especificar estos parámetros en la línea de comandos. Si no lo hace, o se produce un error en la validación de algún parámetro, se le pedirá que proporcione los parámetros requeridos. 

| Nombre de parámetro | DESCRIPCIÓN | Comentario o valor predeterminado | 
| --- | --- | --- | 
| **CloudAdminCredential** | Credencial del administrador de la nube, necesaria para el acceso al punto de conexión con privilegios. | _Obligatorio_ | 
| **AzCredential** | Credenciales de la cuenta de administrador de servicio de Azure Stack. Use las mismas credenciales que para la implementación de Azure Stack. | _Obligatorio_ | 
| **VMLocalCredential** | Las credenciales para la cuenta de administrador local de la VM del proveedor de recursos de MySQL. | _Obligatorio_ | 
| **PrivilegedEndpoint** | Dirección IP o nombre DNS del punto de conexión con privilegios. |  _Obligatorio_ | 
| **DependencyFilesLocalPath** | Ruta de acceso a un recurso compartido local que contiene [mysql-connector-net-6.10.5.msi](https://dev.mysql.com/get/Downloads/Connector-Net/mysql-connector-net-6.10.5.msi). Si se proporciona una de estas rutas de acceso, el archivo de certificados debe colocarse también en este directorio. | _Opcional_ (_obligatorio_ para varios nodos) | 
| **DefaultSSLCertificatePassword** | Contraseña para el certificado .pfx. | _Obligatorio_ | 
| **MaxRetryCount** | Número de veces que quiere volver a intentar cada operación si se produce un error.| 2 | 
| **RetryDuration** | Intervalo de tiempo de expiración entre reintentos, en segundos. | 120 | 
| **Desinstalación** | Se quita el proveedor de recursos y todos los recursos asociados (vea las notas siguientes). | Sin  | 
| **DebugMode** | Impide la limpieza automática en caso de error. | Sin  | 
| **AcceptLicense** | Omite el aviso para aceptar la licencia GPL.  (http://www.gnu.org/licenses/old-licenses/gpl-2.0.html) | | 

> [!NOTE] 
> Las SKU pueden tardar hasta una hora en estar visibles en el portal. No puede crear una base de datos hasta que se cree la SKU. 

## <a name="verify-the-deployment-by-using-the-azure-stack-portal"></a>Comprobación de la implementación mediante Azure Stack Portal 
> [!NOTE] 
>  Cuando finalice la ejecución del script de instalación, deberá actualizar el portal para ver la hoja de administración. 

1. Inicie sesión en el portal de administración como administrador de servicios. 
2. Compruebe que la implementación se realizó correctamente. Vaya a **Grupos de recursos** y seleccione el grupo de recursos **system.\<ubicación\>.mysqladapter**. Compruebe que las cuatro implementaciones se realizaron correctamente:

      ![Comprobación de la implementación del proveedor de recursos de MySQL](./media/azure-stack-mysql-rp-deploy/mysqlrp-verify.png) 


## <a name="next-steps"></a>Pasos siguientes
[Add hosting servers](azure-stack-mysql-resource-provider-hosting-servers.md) (Agregar servidores de hospedaje)
