---
title: Instalación personalizada del entorno de ejecución para la integración de SSIS en Azure
description: En este artículo se describe cómo usar la interfaz de instalación personalizada en el entorno de ejecución para la integración de SSIS en Azure a fin de instalar componentes adicionales o cambiar opciones de configuración.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: swinarko
ms.author: sawinark
manager: mflasko
ms.reviewer: douglasl
ms.custom: seo-lt-2019
ms.date: 02/01/2020
ms.openlocfilehash: e85ef22542fc162648dbfc637892cf7e580c6aac
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2020
ms.locfileid: "76964556"
---
# <a name="customize-setup-for-the-azure-ssis-integration-runtime"></a>Instalación personalizada del entorno de ejecución para la integración de SSIS en Azure

La interfaz de instalación personalizada de la instancia de Integration Runtime para la integración de SSIS en Azure ofrece una interfaz para que agregue su propios pasos de instalación durante el aprovisionamiento o la reconfiguración de dicha instancia. 

La instalación personalizada le permite modificar la configuración o el entorno operativo predeterminado (por ejemplo, para iniciar servicios adicionales de Windows o para que persistan las credenciales de acceso de recursos compartidos de archivos) o instalar componentes adicionales (por ejemplo, ensamblados, controladores o extensiones) en cada nodo de la instancia de Integration Runtime para la integración de SSIS en Azure.

Hay dos maneras de realizar las instalaciones personalizadas en Azure-SSIS IR: las instalaciones rápidas personalizadas sin scripts y las instalaciones estándar personalizadas con scripts.

Con las instalaciones rápidas personalizadas, puede ejecutar algunas configuraciones comunes del sistema o comandos de Windows, o bien instalar algunos componentes adicionales populares o recomendados sin usar ningún script.  

Con las instalaciones estándar personalizadas, debe preparar un script y sus archivos asociados y cargarlos todos juntos en un contenedor de blobs en la cuenta de Azure Storage. A continuación, proporcionará un identificador URI de firma de acceso compartido (SAS) para el contenedor al aprovisionar o volver a configurar su Azure-SSIS IR. Cada nodo de su Azure-SSIS IR descargará el script y sus archivos asociados en el contenedor y ejecutará la instalación personalizada con privilegios elevados. Una vez finalizada la instalación personalizada, cada nodo cargará la salida estándar de la ejecución y otros registros en el contenedor.

Puede instalar componentes gratuitos o sin licencia y de pago o con licencia con instalaciones personalizadas rápidas o estándar. Si es un fabricante de software independiente, consulte nuestra documentación sobre [cómo desarrollar componentes de pago o con licencia para Azure-SSIS IR](how-to-develop-azure-ssis-ir-licensed-components.md).

> [!IMPORTANT]
> Los nodos de la serie v2 de Azure-SSIS Integration Runtime no son adecuados para una instalación personalizada. Por ello, use los nodos de la serie v3 en su lugar.  Si ya usa los nodos de la serie v2, pase a los de la serie v3 tan pronto como sea posible.

## <a name="current-limitations"></a>Limitaciones actuales

Las siguientes limitaciones solo se aplican a las instalaciones estándar personalizadas:

- Si desea utilizar `gacutil.exe` en el script para instalar ensamblados en la memoria caché global de ensamblados (GAC), debe proporcionar `gacutil.exe` como parte de su configuración personalizada o utilizar la copia proporcionada en nuestro contenedor en versión preliminar pública a continuación.

- Si desea hacer referencia a una subcarpeta del script, `msiexec.exe` no admite la notación `.\` para hacer referencia a la carpeta raíz. Use un comando como `msiexec /i "MySubfolder\MyInstallerx64.msi" ...` en lugar de `msiexec /i ".\MySubfolder\MyInstallerx64.msi" ...`.

- Los recursos compartidos administrativos, es decir, los recursos compartidos de red ocultos creados automáticamente por Windows, no se admiten actualmente en Azure-SSIS IR.

- No se admite el controlador ODBC IBM iSeries Access en Azure-SSIS Integration Runtime. Puede ver errores de instalación durante la instalación personalizada. Póngase en contacto con el servicio de soporte técnico de IBM para obtener ayuda.

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Para personalizar el entorno de ejecución de integración de Azure-SSIS, necesita lo siguiente:

- [Suscripción de Azure](https://azure.microsoft.com/)

- [Aprovisionamiento de Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)

- [Una cuenta de Azure Storage](https://azure.microsoft.com/services/storage/). No se necesita para las instalaciones rápidas personalizadas. Para instalaciones estándar personalizadas, cargue y almacene el script de la instalación personalizada y sus archivos asociados en un contenedor de blobs. El proceso de instalación personalizada también carga sus registros de ejecución en el mismo contenedor de blobs.

## <a name="instructions"></a>Instructions

1. Si desea aprovisionar o volver a configurar su Azure-SSIS IR con PowerShell, descargue e instale [Azure PowerShell](/powershell/azure/install-az-ps). En el caso de instalaciones rápidas personalizadas, vaya al paso 4.

1. Prepare el script de instalación personalizada y sus archivos asociados (por ejemplo, archivos .bat, .cmd, .exe, .dll, .msi o. ps1).

   1. Debe tener un archivo de script denominado `main.cmd`, que es el punto de entrada de la configuración personalizada.

   1. Debe asegurarse de que el script se puede ejecutar en modo silencioso, por lo que se recomienda probar primero el script en la máquina local.

   1. Si desea que otros registros generados por otras herramientas (por ejemplo, `msiexec.exe`) se carguen en el contenedor, especifique la variable de entorno predefinida, `CUSTOM_SETUP_SCRIPT_LOG_DIR`, como la carpeta de registro en sus scripts (por ejemplo, `msiexec /i xxx.msi /quiet /lv %CUSTOM_SETUP_SCRIPT_LOG_DIR%\install.log`).

1. Descargue e instale e inicie el [Explorador de Azure Storage](https://storageexplorer.com/).

   1. En **(Local and Attached)** (Local y asociada), haga clic con el botón derecho en **Cuentas de almacenamiento** y seleccione **Conectar a Azure Storage**.

      ![Conectar a Azure Storage](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image1.png)

   1. Seleccione **Usar un nombre y clave de la cuenta de almacenamiento** y, a continuación, seleccione **Siguiente**.

      ![Usar un nombre y clave de la cuenta de almacenamiento](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image2.png)

   1. Escriba el nombre de la cuenta de Azure Storage y la clave, seleccione **Siguiente** y, a continuación, seleccione **Conectar**.

      ![Provisión de nombre y clave de la cuenta de almacenamiento](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image3.png)

   1. En la cuenta conectada de Azure Storage, haga clic con el botón derecho en **Contenedores de blobs**, seleccione **Crear contenedor de blobs** y ponga un nombre al nuevo contenedor.

      ![Creación de un contenedor de blobs](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image4.png)

   1. Seleccione el nuevo contenedor y cargue el script de instalación personalizado y sus archivos asociados. Asegúrese de cargar `main.cmd` en el nivel superior del contenedor, no en cualquier carpeta. Asegúrese también de que el contenedor incluye solo los archivos necesarios de la configuración personalizada, para que descargarlos posteriormente en Azure-SSIS Integration Runtime no lleve demasiado tiempo. El período máximo para la instalación personalizada está establecido actualmente en 45 minutos antes de que se agote el tiempo, y este incluye el tiempo necesario para descargar todos los archivos del contenedor e instalarlos en Azure-SSIS IR. Si es necesario un período más largo, genere una incidencia de soporte técnico.

      ![Carga de archivos en el contenedor de blobs](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image5.png)

   1. Haga clic con el botón derecho en el contenedor y seleccione **Obtener firma de acceso compartido**.

      ![Obtención de una firma de acceso compartido para el contenedor](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image6.png)

   1. Cree el URI de SAS para el contenedor con un tempo de expiración suficientemente largo y con los permisos de lectura, escritura y lista. Necesita el URI de SAS para descargar y ejecutar el script de instalación personalizada y sus archivos asociados siempre que restablezca la imagen o reinicie cualquier nodo de su entorno de ejecución de integración de Azure-SSIS. Se necesita permiso de escritura para cargar los registros de ejecución del programa de instalación.

      > [!IMPORTANT]
      > Asegúrese de que el URI de SAS no expira y que los recursos de instalación personalizada están siempre disponibles durante todo el ciclo de vida del entorno de ejecución de integración de Azure-SSIS durante este período.

      ![Generación de la firma de acceso compartido para el contenedor](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image7.png)

   1. Copie y guarde el URI de SAS del contenedor.

      ![Copia y guardado de la firma de acceso compartido](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image8.png)

1. Al aprovisionar o volver a configurar su Azure-SSIS IR con la interfaz de usuario de Data Factory, puede agregar o quitar las instalaciones personalizadas seleccionando la casilla **Customize your Azure-SSIS Integration Runtime with additional system configurations/component installations** (Personalizar su Azure-SSIS IR con configuraciones del sistema e instalaciones de componentes adicionales) en la sección **Configuración avanzada** del panel de instalación del entorno de ejecución de integración. 

   Si desea agregar instalaciones estándar personalizadas, escriba el URI de SAS del contenedor en el campo **Custom setup container SAS URI** (URI de SAS del contenedor de instalación personalizada). 
   
   Si desea agregar instalaciones rápidas personalizadas, seleccione **Nueva** para abrir el panel **Add express custom setup** (Agregar instalación rápida personalizada) y luego seleccione los tipos en el menú desplegable **Express custom setup type** (Tipo de instalación rápida personalizada):

   1. Si selecciona el tipo **Run cmdkey command** (Ejecutar comando cmdkey), puede conservar las credenciales de acceso de los recursos compartidos de archivos y Azure Files en Azure-SSIS IR especificando el nombre de equipo o dominio de destino, el nombre de cuenta o el nombre de usuario y la clave de cuenta o la contraseña en el los campos **/Add** (/Agregar), **/User** (Usuario) y **/Pass** (/Pasar), respectivamente. Esto es similar a ejecutar el comando [cmdkey](https://docs.microsoft.com/windows-server/administration/windows-commands/cmdkey) de Windows en el equipo local.
   
   1. Si selecciona el tipo **Add environment variable** (Agregar variable de entorno), puede agregar las variables de entorno de Windows que se van a usar en los paquetes que se ejecutan en Azure-SSIS IR escribiendo el nombre y el valor de la variable de entorno en los campos **Nombre de la variable** y **Valor de la variable**, respectivamente. Esto es similar a ejecutar el comando [set](https://docs.microsoft.com/windows-server/administration/windows-commands/set_1) de Windows en el equipo local.

   1. Si selecciona el tipo **Install licensed component** (Instalar componente con licencia), puede seleccionar los componentes integrados de nuestros asociados de ISV en el menú desplegable **Nombre del componente**:

      1. Si selecciona el componente **SentryOne's Task Factory** (Generador de tareas de SentryOne), puede instalar el conjunto de componentes [Task Factory](https://www.sentryone.com/products/task-factory/high-performance-ssis-components) (Generador de tareas) desde SentryOne en su Azure-SSIS IR escribiendo la clave de licencia del producto que compró en el campo **Clave de licencia**. La versión integrada actual es la **2019.4.3**.

      1. Si selecciona el componente **oh22's HEDDA.IO** (HEDDA.IO de oh22), puede instalar el componente de calidad y limpieza de datos [HEDDA.IO](https://hedda.io/ssis-component/) de oh22 en su Azure-SSIS IR después de adquirir su servicio. La versión integrada actual es la **1.0.13**.

      1. Si selecciona el componente **oh22's SQLPhonetics.NET** (SQLPhonetics.NET de oh22), puede instalar el componente de calidad y coincidencia de datos [SQLPhonetics.NET](https://sqlphonetics.oh22.is/sqlphonetics-net-for-microsoft-ssis/) desde oh22 en su Azure-SSIS IR escribiendo la clave de licencia del producto que compró en el campo **Clave de licencia**. La versión integrada actual es la **1.0.43**.
   
   Las instalaciones rápidas personalizadas agregadas aparecerán en la sección **Configuración avanzada**. Para quitarlas, puede activar las casillas de verificación y seleccionar **Eliminar**.

   ![Configuración avanzada con instalaciones personalizadas](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-custom.png)

1. Al aprovisionar o volver a configurar su Azure-SSIS IR con PowerShell, puede agregar o quitar las instalaciones personalizadas mediante la ejecución del cmdlet `Set-AzDataFactoryV2IntegrationRuntime` antes de iniciar su Azure-SSIS IR.
   
   ```powershell
   $ResourceGroupName = "[your Azure resource group name]"
   $DataFactoryName = "[your data factory name]"
   $AzureSSISName = "[your Azure-SSIS IR name]"
   # Custom setup info: Standard/express custom setups
   $SetupScriptContainerSasUri = "" # OPTIONAL to provide a SAS URI of blob container for standard custom setup where your script and its associated files are stored
   $ExpressCustomSetup = "[RunCmdkey|SetEnvironmentVariable|SentryOne.TaskFactory|oh22is.SQLPhonetics.NET|oh22is.HEDDA.IO or leave it empty]" # OPTIONAL to configure an express custom setup without script

   # Add custom setup parameters if you use standard/express custom setups
   if(![string]::IsNullOrEmpty($SetupScriptContainerSasUri))
   {
       Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
           -DataFactoryName $DataFactoryName `
           -Name $AzureSSISName `
           -SetupScriptContainerSasUri $SetupScriptContainerSasUri
   }
   if(![string]::IsNullOrEmpty($ExpressCustomSetup))
   {
       if($ExpressCustomSetup -eq "RunCmdkey")
       {
           $addCmdkeyArgument = "YourFileShareServerName or YourAzureStorageAccountName.file.core.windows.net"
           $userCmdkeyArgument = "YourDomainName\YourUsername or azure\YourAzureStorageAccountName"
           $passCmdkeyArgument = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourPassword or YourAccessKey")
           $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.CmdkeySetup($addCmdkeyArgument, $userCmdkeyArgument, $passCmdkeyArgument)
       }
       if($ExpressCustomSetup -eq "SetEnvironmentVariable")
       {
           $variableName = "YourVariableName"
           $variableValue = "YourVariableValue"
           $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.EnvironmentVariableSetup($variableName, $variableValue)
       }
       if($ExpressCustomSetup -eq "SentryOne.TaskFactory")
       {
           $licenseKey = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourLicenseKey")
           $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup, $licenseKey)
       }
       if($ExpressCustomSetup -eq "oh22is.SQLPhonetics.NET")
       {
           $licenseKey = New-Object Microsoft.Azure.Management.DataFactory.Models.SecureString("YourLicenseKey")
           $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup, $licenseKey)
       }
       if($ExpressCustomSetup -eq "oh22is.HEDDA.IO")
       {
           $setup = New-Object Microsoft.Azure.Management.DataFactory.Models.ComponentSetup($ExpressCustomSetup)
       }
       # Create an array of one or more express custom setups
       $setups = New-Object System.Collections.ArrayList
       $setups.Add($setup)

       Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
           -DataFactoryName $DataFactoryName `
           -Name $AzureSSISName `
           -ExpressCustomSetup $setups
   }
   Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
       -DataFactoryName $DataFactoryName `
       -Name $AzureSSISName `
       -Force
   ```
   
   Después de que finalice la instalación estándar personalizada y se inicie su Azure-SSIS IR, podrá encontrar la salida estándar de `main.cmd` y otros registros de ejecución en la carpeta `main.cmd.log` del contenedor de almacenamiento.

1. Para ver algunos ejemplos de instalaciones estándar personalizadas, conéctese a nuestro contenedor de Versión preliminar pública con el Explorador de Azure Storage.

   1. En **(Local and Attached)** (Local y asociada), haga clic con el botón derecho en **Cuentas de almacenamiento**, seleccione **Conectar a Azure Storage**, luego **Use a connection string or a shared access signature URI** (Use una cadena de conexión o un URI de firma de acceso compartido) y finalmente **Siguiente**.

      ![Conexión a Azure Storage con la firma de acceso compartido](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image9.png)

   1. Seleccione **Use a SAS URI** (Usar un URI de SAS) y escriba el siguiente URI de SAS para el contenedor de versión preliminar pública. Seleccione **Siguiente** y después **Conectar**.

      `https://ssisazurefileshare.blob.core.windows.net/publicpreview?sp=rl&st=2018-04-08T14%3A10%3A00Z&se=2020-04-10T14%3A10%3A00Z&sv=2017-04-17&sig=mFxBSnaYoIlMmWfxu9iMlgKIvydn85moOnOch6%2F%2BheE%3D&sr=c`

      ![Provisión de la firma de acceso compartido para el contenedor](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image10.png)

   1. Seleccione el contenedor de versión preliminar pública conectado y haga doble clic en la carpeta `CustomSetupScript`. En esta carpeta se encuentran los siguientes elementos:

      1. Una carpeta `Sample`, que contiene una instalación personalizada para instalar una tarea básica en cada nodo del entorno de ejecución de integración de Azure-SSIS. La tarea no hace nada, pero se queda en suspensión durante unos segundos. La carpeta también contiene una carpeta `gacutil`, cuyo contenido completo (`gacutil.exe`, `gacutil.exe.config` y `1033\gacutlrc.dll`) se pueden copiar tal cual en el contenedor.

      1. Una carpeta `UserScenarios`, que contiene varios ejemplos de instalaciones personalizadas de escenarios de usuario reales.

      ![Contenido del contenedor de versión preliminar pública](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image11.png)

   1. Haga doble clic en la carpeta `UserScenarios` para buscar los elementos siguientes:

      1. Una carpeta `.NET FRAMEWORK 3.5`, que contiene una instalación personalizada para instalar una versión anterior de .NET Framework que puede ser necesaria para los componentes personalizados en cada nodo del entono de ejecución de integración de Azure-SSIS.

      1. Una carpeta `BCP`, que contiene una instalación personalizada para instalar utilidades de línea de comandos de SQL Server (`MsSqlCmdLnUtils.msi`), incluido el programa de copia masiva (`bcp`), en cada nodo del entorno de ejecución de integración de Azure-SSIS.

      1. Una carpeta `EXCEL`, que contiene un script de instalación personalizada (`main.cmd`) para instalar ensamblados o bibliotecas de C# que puede usar en tareas de script realizar operaciones de lectura o escritura dinámicamente en archivos de Excel en cada nodo de su Azure-SSIS IR. En primer lugar, descargue `ExcelDataReader.dll` desde [aquí](https://www.nuget.org/packages/ExcelDataReader/) y `DocumentFormat.OpenXml.dll` desde [aquí](https://www.nuget.org/packages/DocumentFormat.OpenXml/) y luego cárguelos todos juntos con `main.cmd` en el contenedor. Como alternativa, si solo desea utilizar el Administrador de conexiones, el origen o el destino de Excel estándar, el acceso redistribuible necesario ya está preinstalado en su Azure-SSIS IR, por lo que no necesita ninguna instalación personalizada.
      
      1. Una carpeta `MYSQL ODBC`, que contiene un script de instalación personalizada (`main.cmd`) para instalar los controladores ODBC de MySQL en cada nodo de su Azure-SSIS IR. Esta configuración permite usar el Administrador de conexiones, el origen o el destino de ODBC para conectarse al servidor de MySQL. En primer lugar, descargue las versiones más reciente de 64 y 32 bits de los instaladores de los controladores ODBC de MySQL (por ejemplo, `mysql-connector-odbc-8.0.13-winx64.msi` y `mysql-connector-odbc-8.0.13-win32.msi`) desde [MySQL](https://dev.mysql.com/downloads/connector/odbc/) y luego cárguelos todos juntos con `main.cmd` en el contenedor.

      1. Una carpeta `ORACLE ENTERPRISE`, que contiene un script de instalación personalizada (`main.cmd`) y el archivo de configuración de instalación silenciosa (`client.rsp`) para instalar los conectores de Oracle y el controlador de OCI en cada nodo de Azure-SSIS IR Enterprise Edition. Esta instalación permite usar el Administrador de conexiones, el origen o el destino de Oracle para conectarse al servidor de Oracle. En primer lugar, descargue Microsoft Connectors v5.0 para Oracle (`AttunitySSISOraAdaptersSetup.msi` y `AttunitySSISOraAdaptersSetup64.msi`) desde el [Centro de descarga de Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=55179) y la versión más reciente del cliente de Oracle (por ejemplo, `winx64_12102_client.zip`) desde [Oracle](https://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-win64-download-2297732.html) y luego cárguelos todos junto con `main.cmd` y `client.rsp` en el contenedor. Si usa TNS para conectar con Oracle, también debe descargar `tnsnames.ora`, editarlo y cargarlo en el contenedor, para que se pueda copiar en la carpeta de instalación de Oracle durante el proceso de instalación.

      1. Una carpeta `ORACLE STANDARD ADO.NET`, que contiene un script de instalación personalizada (`main.cmd`) para instalar el controlador ODP.NET de Oracle en cada nodo del entorno de ejecución de integración de Azure-SSIS. Esta instalación permite usar el Administrador de conexiones, el origen o el destino de ADO.NET para conectarse al servidor Oracle. En primer lugar, descargue el controlador de ODP.NET de Oracle más reciente (por ejemplo, `ODP.NET_Managed_ODAC122cR1.zip`) desde [Oracle](https://www.oracle.com/technetwork/database/windows/downloads/index-090165.html) y luego cárguelo junto con `main.cmd` en el contenedor.
       
      1. Una carpeta `ORACLE STANDARD ODBC`, que contiene un script de configuración personalizada (`main.cmd`) para instalar el controlador ODBC de Oracle y configurar DSN en cada nodo de Azure-SSIS Integration Runtime. Esta configuración le permite usar el administrador de conexiones, el origen o el destino de ODBC, o el administrador de conexiones o el origen de Power Query con orígenes de datos ODBC para conectarse a un servidor de Oracle. Primero, descargue la versión más reciente de Oracle Instant Client (paquete Basic o Basic Lite) y del paquete ODBC: por ejemplo, los paquetes de 64 bits de [aquí](https://www.oracle.com/technetwork/topics/winx64soft-089540.html) (paquete Basic: `instantclient-basic-windows.x64-18.3.0.0.0dbru.zip`, paquete Basic Lite: `instantclient-basiclite-windows.x64-18.3.0.0.0dbru.zip`, paquete ODBC: `instantclient-odbc-windows.x64-18.3.0.0.0dbru.zip`), o los paquetes de 32 bits de [aquí](https://www.oracle.com/technetwork/topics/winsoft-085727.html) (paquete Basic: `instantclient-basic-nt-18.3.0.0.0dbru.zip`, paquete Basic Lite: `instantclient-basiclite-nt-18.3.0.0.0dbru.zip`, paquete ODBC: `instantclient-odbc-nt-18.3.0.0.0dbru.zip`) y luego cárguelos todos juntos con `main.cmd` en el contenedor.

      1. Una carpeta `ORACLE STANDARD OLEDB`, que contiene un script de instalación personalizada (`main.cmd`) para instalar el controlador OLEDB de Oracle en cada nodo del entorno de ejecución de integración de Azure-SSIS. Esta instalación permite usar el Administrador de conexiones, el origen o el destino de OLEDB para conectarse al servidor de Oracle. En primer lugar, descargue el controlador de OLEDB de Oracle más reciente (por ejemplo, `ODAC122010Xcopy_x64.zip`) desde [Oracle](https://www.oracle.com/partners/campaign/index-090165.html) y luego cárguelo junto con `main.cmd` en el contenedor.

      1. Una carpeta `POSTGRESQL ODBC`, que contiene un script de instalación personalizada (`main.cmd`) para instalar los controladores ODBC de PostgreSQL en cada nodo de su Azure-SSIS IR. Esta configuración permite usar el Administrador de conexiones, el origen o el destino de ODBC para conectarse al servidor de PostgreSQL. En primer lugar, descargue las versiones más reciente de 64 y 32 bits de los instaladores de los controladores ODBC de PostgreSQL (por ejemplo, `psqlodbc_x64.msi` y `psqlodbc_x86.msi`) desde [PostgreSQL](https://www.postgresql.org/ftp/odbc/versions/msi/) y luego cárguelos todos juntos con `main.cmd` en el contenedor.

      1. Una carpeta `SAP BW`, que contiene un script de instalación personalizada (`main.cmd`) para instalar el ensamblado del conector SAP .NET (`librfc32.dll`) en cada nodo de la edición Enterprise de Azure-SSIS IR. Esta configuración permite usar el Administrador de conexiones, el origen o el destino de SAP BW para conectarse al servidor de SAP BW. En primer lugar, cargue la versión de 64 bits o de 32 bits de `librfc32.dll` desde la carpeta de instalación de SAP junto con `main.cmd` en el contenedor. El script copia entonces el ensamblado de SAP en la carpeta `%windir%\SysWow64` o `%windir%\System32` durante la instalación.

      1. Una carpeta `STORAGE`, que contiene una instalación personalizada para instalar Azure PowerShell en cada nodo del entorno de ejecución de integración de Azure-SSIS. Este programa de instalación le permite implementar y ejecutar paquetes de SSIS que ejecutan [scripts de PowerShell para manipular la cuenta de Azure Storage](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-use-blobs-powershell). Copie `main.cmd`, un `AzurePowerShell.msi` de ejemplo (o use la versión más reciente) y `storage.ps1` en su contenedor. Use PowerShell.dtsx como plantilla para los paquetes. La plantilla de paquetes combina una [tarea de descarga de blobs de Azure](https://docs.microsoft.com/sql/integration-services/control-flow/azure-blob-download-task), que descarga `storage.ps1` como un script de PowerShell modificable, y una [tarea de ejecución de proceso](https://blogs.msdn.microsoft.com/ssis/2017/01/26/run-powershell-scripts-in-ssis/) que ejecuta el script en cada nodo.

      1. Una carpeta `TERADATA`, que contiene un script de instalación personalizada (`main.cmd`), su archivo asociado (`install.cmd`) y los paquetes del instalador (`.msi`). Estos archivos instalan los conectores Teradata, la API TPT y el controlador ODBC en cada nodo de la edición Enterprise de Integration Runtime para la integración de SSIS en Azure. Esta configuración permite usar el Administrador de conexiones, el origen o el destino de Teradata para conectarse al servidor de Teradata. En primer lugar, descargue el archivo ZIP de Teradata Tools and Utilities 15.x (por ejemplo, `TeradataToolsAndUtilitiesBase__windows_indep.15.10.22.00.zip`) desde [Teradata](http://partnerintelligence.teradata.com) y luego cárguelo junto con los archivos anteriores `.cmd` y `.msi` en el contenedor.

      1. Una carpeta `ZULU OPENJDK`, que contiene un script de instalación personalizada (`main.cmd`) y un archivo PowerShell (`install_openjdk.ps1`) para instalar Zulu OpenJDK en cada nodo de su Azure-SSIS IR. Esta configuración permite usar conectores de Azure Data Lake Store o Flexible File para procesar archivos ORC o Parquet. Consulte [aquí](https://docs.microsoft.com/sql/integration-services/azure-feature-pack-for-integration-services-ssis?view=sql-server-ver15#dependency-on-java) para más información. En primer lugar, descargue la versión más reciente de Zulu OpenJDK (por ejemplo, `zulu8.33.0.1-jdk8.0.192-win_x64.zip`) desde [aquí](https://www.azul.com/downloads/zulu/zulu-windows/) y luego cárguelo junto con `main.cmd` y `install_openjdk.ps1` en el contenedor.

      ![Carpetas en la carpeta de escenarios de usuario](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image12.png)

   1. Para probar estos ejemplos de instalación personalizada, copie y pegue el contenido de la carpeta seleccionada en el contenedor.
   
      Al aprovisionar o volver a configurar su Azure-SSIS IR con la interfaz de usuario de Data Factory, seleccione la casilla **Customize your Azure-SSIS Integration Runtime with additional system configurations/component installations** (Personalizar su Azure-SSIS IR con configuraciones del sistema e instalaciones de componentes adicionales) en la sección **Configuración avanzada** y escriba el URI de SAS del contenedor en el campo **Custom setup container SAS URI** (URI de SAS del contenedor de instalación personalizada).
   
      Al aprovisionar o volver a configurar el entorno de ejecución de integración de Azure-SSIS con PowerShell, ejecute el cmdlet `Set-AzDataFactoryV2IntegrationRuntime` con el URI de SAS del contenedor como el valor para el parámetro `SetupScriptContainerSasUri`.

## <a name="next-steps"></a>Pasos siguientes

-   [Enterprise Edition of the Azure-SSIS Integration Runtime](how-to-configure-azure-ssis-ir-enterprise-edition.md) (Edición Enterprise para la instancia de Integration Runtime para la integración de SSIS en Azure)

-   [How to develop paid or licensed custom components for the Azure-SSIS integration runtime](how-to-develop-azure-ssis-ir-licensed-components.md) (Desarrollo de componentes personalizados de pago o con licencia para el entorno de ejecución de integración de Azure-SSIS)
