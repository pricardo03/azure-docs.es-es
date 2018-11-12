---
title: Instalación personalizada del entorno de ejecución para la integración de SSIS en Azure | Microsoft Docs
description: En este artículo se describe cómo usar la interfaz de instalación personalizada en el entorno de ejecución para la integración de SSIS en Azure a fin de instalar componentes adicionales o cambiar opciones de configuración.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/31/2018
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: df020fc3a4e2f57730dea7329b08e1e46660e610
ms.sourcegitcommit: f0c2758fb8ccfaba76ce0b17833ca019a8a09d46
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2018
ms.locfileid: "51037046"
---
# <a name="customize-setup-for-the-azure-ssis-integration-runtime"></a>Instalación personalizada del entorno de ejecución para la integración de SSIS en Azure

La interfaz de instalación personalizada de la instancia de Integration Runtime para la integración de SSIS en Azure ofrece una interfaz para que agregue su propios pasos de instalación durante el aprovisionamiento o la reconfiguración de dicha instancia. La instalación personalizada le permite modificar la configuración o el entorno operativo predeterminado (por ejemplo, para iniciar servicios adicionales de Windows o para que persistan las credenciales de acceso de recursos compartidos de archivos) o instalar componentes adicionales (por ejemplo, ensamblados, controladores o extensiones) en cada nodo de la instancia de Integration Runtime para la integración de SSIS en Azure.

Para llevar a cabo la configuración personalizada, debe preparar un script y sus archivos asociados y cargarlos en un contenedor de blobs en la cuenta de Azure Storage. Proporcione un identificador URI de firma de acceso compartido (SAS) para el contenedor al aprovisionar o volver a configurar el entorno de ejecución de integración de Azure-SSIS. Cada nodo del entorno de ejecución de integración de Azure-SSIS descarga entonces el script y sus archivos asociados en el contenedor y ejecuta la instalación personalizada con privilegios elevados. Una vez finalizada la instalación personalizada, cada nodo carga la salida estándar de la ejecución y otros registros en el contenedor.

Puede instalar tanto componentes libres o sin licencia como componentes de pago o con licencia. Si es un fabricante de software independiente, consulte [How to develop paid or licensed components for the Azure-SSIS IR](how-to-develop-azure-ssis-ir-licensed-components.md) (Desarrollo de componentes de pago o con licencia para el entorno de ejecución de integración de Azure-SSIS).


## <a name="current-limitations"></a>Limitaciones actuales

-   Si desea utilizar `gacutil.exe` para instalar ensamblados en la caché global de ensamblados (GAC), debe proporcionar `gacutil.exe` como parte de su configuración personalizada o utilizar la copia proporcionada en el contenedor en versión preliminar pública.

-   Si desea hacer referencia a una subcarpeta del script, `msiexec.exe` no admite la notación `.\` para hacer referencia a la carpeta raíz. Use un comando como `msiexec /i "MySubfolder\MyInstallerx64.msi" ...` en lugar de `msiexec /i ".\MySubfolder\MyInstallerx64.msi" ...`.

-   Si tiene que unir una instancia de Integration Runtime para la integración de SSIS en Azure con instalación personalizada a una red virtual, tiene que ser la red virtual de Azure Resource Manager. No se admite la red virtual clásica.

-   Actualmente no se admite el uso compartido administrativo en la instancia de Integration Runtime para la integración de SSIS en Azure.

## <a name="prerequisites"></a>Requisitos previos

Para personalizar el entorno de ejecución de integración de Azure-SSIS, necesita lo siguiente:

-   [Suscripción de Azure](https://azure.microsoft.com/)

-   [Una instancia de Azure SQL Database o servidor de Instancia administrada](https://ms.portal.azure.com/#create/Microsoft.SQLServer)

-   [Aprovisionamiento del entorno de ejecución de integración de Azure-SSIS](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure)

-   [Una cuenta de Azure Storage](https://azure.microsoft.com/services/storage/). Para la instalación personalizada, cargue y almacene el script de la instalación personalizada y sus archivos asociados en un contenedor de blobs. El proceso de instalación personalizada también carga sus registros de ejecución en el mismo contenedor de blobs.

## <a name="instructions"></a>Instrucciones

1.  Descargue e instale [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) (versión 5.4 o posterior).

1.  Prepare el script de instalación personalizada y sus archivos asociados (por ejemplo, archivos .bat, .cmd, .exe, .dll, .msi o. ps1).

    1.  Debe tener un archivo de script denominado `main.cmd`, que es el punto de entrada de la configuración personalizada.

    1.  Si desea que otros registros generados por otras herramientas (por ejemplo, `msiexec.exe`) se carguen en el contenedor, especifique la variable de entorno predefinida, `CUSTOM_SETUP_SCRIPT_LOG_DIR`, como la carpeta de registro en sus scripts (por ejemplo, `msiexec /i xxx.msi /quiet /lv %CUSTOM_SETUP_SCRIPT_LOG_DIR%\install.log`).

1.  Descargue e instale e inicie el [Explorador de Azure Storage](http://storageexplorer.com/).

    1.  En **(Local and Attached)** (Local y asociada), haga clic con el botón derecho en **Cuentas de almacenamiento** y seleccione **Conectar a Azure Storage**.

       ![Conectar a Azure Storage](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image1.png)

    1.  Seleccione **Usar un nombre y clave de la cuenta de almacenamiento** y, a continuación, seleccione **Siguiente**.

       ![Usar un nombre y clave de la cuenta de almacenamiento](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image2.png)

    1.  Escriba el nombre de la cuenta de Azure Storage y la clave, seleccione **Siguiente** y, a continuación, seleccione **Conectar**.

       ![Provisión de nombre y clave de la cuenta de almacenamiento](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image3.png)

    1.  En la cuenta conectada de Azure Storage, haga clic con el botón derecho en **Contenedores de blobs**, seleccione **Crear contenedor de blobs** y ponga un nombre al nuevo contenedor.

       ![Creación de un contenedor de blobs](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image4.png)

    1.  Seleccione el nuevo contenedor y cargue el script de instalación personalizado y sus archivos asociados. Asegúrese de cargar `main.cmd` en el nivel superior del contenedor, no en cualquier carpeta. 

       ![Carga de archivos en el contenedor de blobs](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image5.png)

    1.  Haga clic con el botón derecho en el contenedor y seleccione **Obtener firma de acceso compartido**.

       ![Obtención de una firma de acceso compartido para el contenedor](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image6.png)

    1.  Cree el URI de SAS para el contenedor con un tempo de expiración suficientemente largo y con los permisos de lectura, escritura y lista. Necesita el URI de SAS para descargar y ejecutar el script de instalación personalizada y sus archivos asociados siempre que restablezca la imagen o reinicie cualquier nodo de su entorno de ejecución de integración de Azure-SSIS. Se necesita permiso de escritura para cargar los registros de ejecución del programa de instalación.

        > [!IMPORTANT]
        > Asegúrese de que el URI de SAS no expira y que los recursos de instalación personalizada están siempre disponibles durante todo el ciclo de vida del entorno de ejecución de integración de Azure-SSIS durante este período.

       ![Generación de la firma de acceso compartido para el contenedor](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image7.png)

    1.  Copie y guarde el URI de SAS del contenedor.

       ![Copia y guardado de la firma de acceso compartido](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image8.png)

    1.  Al aprovisionar o volver a configurar Azure-SSIS IR con la interfaz de usuario de Data Factory, antes de iniciar Azure-SSIS IR, escriba el URI de SAS del contenedor en el campo correspondiente en el panel **Configuración avanzada**:

       ![Especificar la firma de acceso compartido](media/tutorial-create-azure-ssis-runtime-portal/advanced-settings.png)

       Al aprovisionar o volver a configurar el entorno de ejecución de integración de Azure-SSIS con PowerShell, antes de iniciar el entorno de ejecución de integración de Azure-SSIS, ejecute el cmdlet `Set-AzureRmDataFactoryV2IntegrationRuntime` con el URI de SAS del contenedor como el valor para el nuevo parámetro `SetupScriptContainerSasUri`. Por ejemplo: 

       ```powershell
       Set-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName `
                                                  -Name $MyAzureSsisIrName `
                                                  -ResourceGroupName $MyResourceGroupName `
                                                  -SetupScriptContainerSasUri $MySetupScriptContainerSasUri

       Start-AzureRmDataFactoryV2IntegrationRuntime -DataFactoryName $MyDataFactoryName `
                                                    -Name $MyAzureSsisIrName `
                                                    -ResourceGroupName $MyResourceGroupName
       ```

    1.  Después de que finalice el programa de instalación personalizado y se inicie el entorno de ejecución de integración de Azure-SSIS, podrá encontrar la salida estándar de `main.cmd` y otros registros de ejecución en la carpeta `main.cmd.log` de su contenedor de almacenamiento.

1.  Para ver otros ejemplos de instalación personalizada, conéctese al contenedor de vista preliminar pública con el Explorador de Azure Storage.

    a.  En **(Local and Attached)** (Local y asociada), haga clic con el botón derecho en **Cuentas de almacenamiento**, seleccione **Conectar a Azure Storage**, luego **Use a connection string or a shared access signature URI** (Use una cadena de conexión o un URI de firma de acceso compartido) y finalmente **Siguiente**.

       ![Conexión a Azure Storage con la firma de acceso compartido](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image9.png)

    b.  Seleccione **Use a SAS URI** (Usar un URI de SAS) y escriba el siguiente URI de SAS para el contenedor de versión preliminar pública. Seleccione **Siguiente** y luego **Conectar**.

       `https://ssisazurefileshare.blob.core.windows.net/publicpreview?sp=rl&st=2018-04-08T14%3A10%3A00Z&se=2020-04-10T14%3A10%3A00Z&sv=2017-04-17&sig=mFxBSnaYoIlMmWfxu9iMlgKIvydn85moOnOch6%2F%2BheE%3D&sr=c`

       ![Provisión de la firma de acceso compartido para el contenedor](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image10.png)

    c. Seleccione el contenedor de versión preliminar pública conectado y haga doble clic en la carpeta `CustomSetupScript`. En esta carpeta se encuentran los siguientes elementos:

       1. Una carpeta `Sample`, que contiene una instalación personalizada para instalar una tarea básica en cada nodo del entorno de ejecución de integración de Azure-SSIS. La tarea no hace nada, pero se queda en suspensión durante unos segundos. La carpeta también contiene una carpeta `gacutil`, que contiene `gacutil.exe`. Además, `main.cmd` contiene comentarios para conservar las credenciales de acceso para recursos compartidos de archivos.

       1. Una carpeta `UserScenarios`, que contiene varias configuraciones personalizadas para escenarios de usuario real.

    ![Contenido del contenedor de versión preliminar pública](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image11.png)

    d. Haga doble clic en la carpeta `UserScenarios`. En esta carpeta se encuentran los siguientes elementos:

       1. Una carpeta `.NET FRAMEWORK 3.5`, que contiene una instalación personalizada para instalar una versión anterior de .NET Framework que puede ser necesaria para los componentes personalizados en cada nodo del entono de ejecución de integración de Azure-SSIS.

       1. Una carpeta `AAS`, que contiene una configuración personalizada para instalar las bibliotecas cliente en cada nodo de Azure-SSIS IR que permiten a las tareas de Analysis Services conectarse a la instancia de Azure Analysis Services (AAS) mediante la autenticación de entidad de servicio. En primer lugar, descargue la versión más reciente de los instaladores de Windows o las bibliotecas cliente de **MSOLAP (amd64)** y **AMO** (por ejemplo, `x64_15.0.900.108_SQL_AS_OLEDB.msi` y `x64_15.0.900.108_SQL_AS_AMO.msi`) desde [aquí](https://docs.microsoft.com/azure/analysis-services/analysis-services-data-providers) y luego cárguelos todos juntos con `main.cmd` en el contenedor.  

       1. Una carpeta `BCP`, que contiene una instalación personalizada para instalar utilidades de línea de comandos de SQL Server (`MsSqlCmdLnUtils.msi`), incluido el programa de copia masiva (`bcp`), en cada nodo del entorno de ejecución de integración de Azure-SSIS.

       1. Una carpeta `EXCEL`, que contiene una instalación personalizada para instalar ensamblados de código abierto (`DocumentFormat.OpenXml.dll`, `ExcelDataReader.DataSet.dll` y `ExcelDataReader.dll`) en cada nodo del entorno de ejecución de integración de Azure-SSIS.

       1. Una carpeta `MSDTC`, que contiene una instalación personalizada para modificar las configuraciones de red y seguridad para el servicio del Coordinador de transacciones distribuidas de Microsoft (MSDTC) en cada nodo del entorno de ejecución de integración de Azure-SSIS. Para asegurarse de que MSDTC se ha iniciado, agregue la tarea Ejecutar proceso al principio del flujo de control de los paquetes para ejecutar el comando siguiente: `%SystemRoot%\system32\cmd.exe /c powershell -Command "Start-Service MSDTC"` 

       1. Una carpeta `ORACLE ENTERPRISE`, que contiene un script de instalación personalizada (`main.cmd`) y el archivo de configuración de instalación silenciosa (`client.rsp`) para instalar los conectores de Oracle y el controlador de OCI en cada nodo de Azure-SSIS IR Enterprise Edition. Este programa de instalación le permite utilizar el administrador de conexiones, el origen y el destino de Oracle. En primer lugar, descargue Microsoft Connectors v5.0 para Oracle (`AttunitySSISOraAdaptersSetup.msi` y `AttunitySSISOraAdaptersSetup64.msi`) desde el [Centro de descarga de Microsoft](https://www.microsoft.com/en-us/download/details.aspx?id=55179) y la versión más reciente del cliente de Oracle (por ejemplo, `winx64_12102_client.zip`) desde [Oracle](http://www.oracle.com/technetwork/database/enterprise-edition/downloads/database12c-win64-download-2297732.html) y luego cárguelos todos junto con `main.cmd` y `client.rsp` en el contenedor. Si usa TNS para conectar con Oracle, también debe descargar `tnsnames.ora`, editarlo y cargarlo en el contenedor, para que se pueda copiar en la carpeta de instalación de Oracle durante el proceso de instalación.

       1. Una carpeta `ORACLE STANDARD`, que contiene un script de instalación personalizada (`main.cmd`) para instalar el controlador ODP.NET de Oracle en cada nodo del entorno de ejecución de integración de Azure-SSIS. Este programa de instalación le permite utilizar el administrador de conexiones, el origen y el destino de ADO.NET. En primer lugar, descargue el controlador de Oracle ODP.NET más reciente —por ejemplo, `ODP.NET_Managed_ODAC122cR1.zip`— de [Oracle](http://www.oracle.com/technetwork/database/windows/downloads/index-090165.html) y, a continuación, cárguelo junto con `main.cmd` en su contenedor.

       1. Una carpeta `SAP BW`, que contiene un script de instalación personalizada (`main.cmd`) para instalar el ensamblado del conector SAP .NET (`librfc32.dll`) en cada nodo de la edición Enterprise de Integration Runtime para la integración de SSIS en Azure. Este programa de instalación le permite utilizar el administrador de conexiones, el origen y el destino de SAP BW. En primer lugar, cargue la versión de 64 bits o de 32 bits de `librfc32.dll` desde la carpeta de instalación de SAP en el contenedor, junto con `main.cmd`. El script copia entonces el ensamblado de SAP en la carpeta `%windir%\SysWow64` o `%windir%\System32` durante la instalación.

       1. Una carpeta `STORAGE`, que contiene una instalación personalizada para instalar Azure PowerShell en cada nodo del entorno de ejecución de integración de Azure-SSIS. Este programa de instalación le permite implementar y ejecutar paquetes de SSIS que ejecutan [scripts de PowerShell para manipular la cuenta de Azure Storage](https://docs.microsoft.com/azure/storage/blobs/storage-how-to-use-blobs-powershell). Copie `main.cmd`, un `AzurePowerShell.msi` de ejemplo (o instale la versión más reciente) y `storage.ps1` en su contenedor. Use PowerShell.dtsx como plantilla para los paquetes. La plantilla de paquetes combina una [tarea de descarga de blobs de Azure](https://docs.microsoft.com/sql/integration-services/control-flow/azure-blob-download-task), que descarga `storage.ps1` como un script de PowerShell modificable, y una [tarea de ejecución de proceso](https://blogs.msdn.microsoft.com/ssis/2017/01/26/run-powershell-scripts-in-ssis/) que ejecuta el script en cada nodo.

       1. Una carpeta `TERADATA`, que contiene un script de instalación personalizada (`main.cmd`), su archivo asociado (`install.cmd`) y los paquetes del instalador (`.msi`). Estos archivos instalan conectores Teradata, la API TPT y el controlador ODBC en cada nodo de la edición Enterprise de Integration Runtime para la integración de SSIS en Azure. Este programa de instalación le permite utilizar el administrador de conexiones, el origen y el destino de Teradata. En primer lugar, descargue el archivo ZIP de 15.x de Teradata Tools and Utilities (por ejemplo, `TeradataToolsAndUtilitiesBase__windows_indep.15.10.22.00.zip`) desde [Teradata](http://partnerintelligence.teradata.com) y, a continuación, cárguelo junto con los archivos anteriores `.cmd` y `.msi` en su contenedor.

    ![Carpetas en la carpeta de escenarios de usuario](media/how-to-configure-azure-ssis-ir-custom-setup/custom-setup-image12.png)

    e. Para probar estos ejemplos de instalación personalizada, copie y pegue el contenido de la carpeta seleccionada en el contenedor. Al aprovisionar o volver a configurar el entorno de ejecución de integración de Azure-SSIS con PowerShell, ejecute el cmdlet `Set-AzureRmDataFactoryV2IntegrationRuntime` con el URI de SAS del contenedor como el valor para el nuevo parámetro `SetupScriptContainerSasUri`.

## <a name="next-steps"></a>Pasos siguientes

-   [Enterprise Edition of the Azure-SSIS Integration Runtime](how-to-configure-azure-ssis-ir-enterprise-edition.md) (Edición Enterprise para la instancia de Integration Runtime para la integración de SSIS en Azure)

-   [How to develop paid or licensed custom components for the Azure-SSIS integration runtime](how-to-develop-azure-ssis-ir-licensed-components.md) (Desarrollo de componentes personalizados de pago o con licencia para el entorno de ejecución de integración de Azure-SSIS)
