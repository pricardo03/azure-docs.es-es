---
title: Configuración del entorno de ejecución de integración autohospedado como proxy para SSIS
description: Aprenda a configurar el entorno de ejecución de integración autohospedado como un proxy para Azure-SSIS Integration Runtime.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: mflasko
ms.custom: seo-lt-2019
ms.date: 12/23/2019
ms.openlocfilehash: 48d4df5684c84e195810439912dd610f5af364d4
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2020
ms.locfileid: "76964488"
---
# <a name="configure-self-hosted-ir-as-a-proxy-for-azure-ssis-ir-in-adf"></a>Configuración del entorno de ejecución de integración autohospedado como un proxy para Azure-SSIS IR en Azure Data Factory

En este artículo se describe cómo ejecutar paquetes de SQL Server Integration Services (SSIS) en Azure-SSIS Integration Runtime (IR) en Azure Data Factory (ADF) con el modo de entorno de ejecución de integración autohospedado configurado como proxy.  Esta característica permite acceder a los datos locales sin necesidad de [unir la instancia de Azure-SSIS IR a una red virtual](https://docs.microsoft.com/azure/data-factory/join-azure-ssis-integration-runtime-virtual-network).  Resulta útil cuando la red corporativa tiene una directiva de configuración o restrictiva demasiado compleja para que pueda insertar la instancia de Azure-SSIS IR en ella.

Esta característica dividirá el paquete que contiene una tarea Flujo de datos con un origen de datos local en dos tareas de almacenamiento provisional: la primera, que se ejecuta en el entorno de ejecución de integración autohospedado, moverá primero los datos desde el origen de datos local a un área provisional en Azure Blob Storage, mientras que la segunda, que se ejecuta en Azure-SSIS IR moverá los datos del área de almacenamiento provisional al destino de datos previsto.

Esta característica también proporciona otras ventajas y funcionalidades, ya que le permite aprovisionar el entorno de ejecución de integración autohospedado en regiones que todavía no admiten Azure-SSIS IR o permite la dirección IP estática pública de la instancia del entorno de ejecución de integración autohospedado en el firewall de los orígenes de datos, entre otras ventajas.

## <a name="prepare-self-hosted-ir"></a>Preparación de una instancia de IR autohospedada

Para usar esta característica, primero debe crear una instancia de ADF y aprovisionar en él Azure-SSIS IR, si aún no lo ha hecho, para lo que debe seguir el artículo sobre [cómo aprovisionar una instancia de Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure).

Después, deberá aprovisionar el entorno de ejecución de integración autohospedado en la misma instancia de ADF en la que se aprovisiona la instancia de Azure-SSIS IR, para lo que debe seguir el artículo sobre [cómo crear un entorno de ejecución de integración autohospedado](https://docs.microsoft.com/azure/data-factory/create-self-hosted-integration-runtime).

Por último, tendrá que descargar e instalar la versión más reciente del entorno de ejecución de integración autohospedado, así como los controladores y el entorno de ejecución adicionales, en la máquina virtual de Azure o en el equipo local, tal como se indica a continuación:
- Descargue e instale [aquí](https://www.microsoft.com/download/details.aspx?id=39717) la versión más reciente del IR autohospedado.
- Si usa conectores de OLEDB en los paquetes, descargue e instale los controladores OLEDB apropiados en el mismo equipo donde está instalado el IR autohospedado si aún no lo ha hecho.  Si utiliza la versión anterior del controlador OLEDB para SQL Server (SQLNCLI), puede descargar la versión de 64 bits [aquí.](https://www.microsoft.com/download/details.aspx?id=50402)  Si utiliza la versión más reciente del controlador OLEDB para SQL Server (MSOLEDBSQL), puede descargar la versión de 64 bits [aquí.](https://www.microsoft.com/download/details.aspx?id=56730)  Si utiliza controladores OLEDB para otros sistemas de base de datos, como PostgreSQL, MySQL, Oracle, etc., puede descargar la versión de 64 bits en sus respectivos sitios web.
- Descargue e instale el entorno de ejecución de Visual C++ (VC) en la misma máquina donde está instalado el IR autohospedado si aún no lo ha hecho.  Puede descargar la versión de 64 bits [aquí](https://www.microsoft.com/download/details.aspx?id=40784).

## <a name="prepare-azure-blob-storage-linked-service-for-staging"></a>Preparación del servicio vinculado de Azure Blob Storage para almacenamiento provisional

Cree un servicio vinculado de Azure Blob Storage en la misma instancia de ADF donde está aprovisionado Azure-SSIS IR si aún no lo ha hecho; para ello, siga el artículo sobre [cómo crear un servicio vinculado de ADF](https://docs.microsoft.com/azure/data-factory/quickstart-create-data-factory-portal#create-a-linked-service).  Asegúrese de lo siguiente:
- **Azure Blob Storage** está seleccionado en **Almacén de datos**
- **AutoResolveIntegrationRuntime** está seleccionado en **Connect via integration runtime** (Conectar mediante IR)
- **Clave de cuenta**/**URI SAS**/**Entidad de servicio** está seleccionado en **Método de autenticación**

>[!TIP]
>Al seleccionar **Entidad de servicio** , conceda al menos el **rol de colaborador de datos de Blob Storage**. Para obtener más información, consulte [conector de Azure Blob Storage](connector-azure-blob-storage.md#linked-service-properties).

![Preparación del servicio vinculado de Azure Blob Storage para almacenamiento provisional](media/self-hosted-integration-runtime-proxy-ssis/shir-azure-blob-storage-linked-service.png)

## <a name="configure-azure-ssis-ir-with-self-hosted-ir-as-a-proxy"></a>Configuración de Azure-SSIS IR con el entorno de ejecución de integración autohospedado como un proxy

Cuando haya preparado el servicio vinculado de Azure Blob Storage y el entorno de ejecución de integración autohospedado para el almacenamiento provisional, puede configurar la instancia nueva o existente de Azure-SSIS IR con el entorno de ejecución de integración autohospedado como proxy en el portal o la aplicación de ADF.  Si la instancia de Azure-SSIS IR existente está en ejecución, deténgala antes de hacerlo y reiníciela después.

1. En el panel de configuración del entorno de ejecución de integración, avance por las secciones **Configuración general** y **Configuración de SQL** seleccionando el botón **Siguiente**. 

1. En la sección **Configuración avanzada**:

   1. Active la casilla **Set up Self-Hosted Integration Runtime as a proxy for your Azure-SSIS Integration Runtime** (Configurar el entorno de ejecución de integración autohospedado como proxy para su instancia de Azure-SSIS Integration Runtime). 

   1. En **Self-Hosted Integration Runtime** (Entorno de ejecución de integración autohospedado), seleccione la instancia de IR autohospedado existente como proxy para Azure-SSIS IR.

   1. En **Staging Storage Linked Service** (Servicio vinculado de almacenamiento provisional), seleccione el servicio vinculado de Azure Blob Storage existente o cree uno para el almacenamiento provisional.

   1. En **Staging Path** (Ruta de acceso provisional), especifique un contenedor de blobs en la instancia seleccionada de Azure Blob Storage o deje este campo vacío para usar uno predeterminado para el almacenamiento provisional.

   1. Seleccione **Continuar**.

   ![Configuración avanzada con IR autohospedado](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-shir.png)

También puede configurar la instancia de Azure-SSIS IR nuevo o existente con IR autohospedado como proxy mediante PowerShell.

```powershell
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$AzureSSISName = "[your Azure-SSIS IR name]"
# Self-hosted integration runtime info - This can be configured as a proxy for on-premises data access 
$DataProxyIntegrationRuntimeName = "" # OPTIONAL to configure a proxy for on-premises data access 
$DataProxyStagingLinkedServiceName = "" # OPTIONAL to configure a proxy for on-premises data access 
$DataProxyStagingPath = "" # OPTIONAL to configure a proxy for on-premises data access 

# Add self-hosted integration runtime parameters if you configure a proxy for on-premises data accesss
if(![string]::IsNullOrEmpty($DataProxyIntegrationRuntimeName) -and ![string]::IsNullOrEmpty($DataProxyStagingLinkedServiceName))
{
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
        -DataFactoryName $DataFactoryName `
        -Name $AzureSSISName `
        -DataProxyIntegrationRuntimeName $DataProxyIntegrationRuntimeName `
        -DataProxyStagingLinkedServiceName $DataProxyStagingLinkedServiceName

    if(![string]::IsNullOrEmpty($DataProxyStagingPath))
    {
        Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
            -DataFactoryName $DataFactoryName `
            -Name $AzureSSISName `
            -DataProxyStagingPath $DataProxyStagingPath
    }
}
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $DataFactoryName `
    -Name $AzureSSISName `
    -Force
```

## <a name="enable-ssis-packages-to-connect-by-proxy"></a>Habilitación de paquetes SSIS para conectarse por proxy

Con la extensión de SSDT más reciente con proyectos de SSIS para Visual Studio que se puede descargar [aquí](https://marketplace.visualstudio.com/items?itemName=SSIS.SqlServerIntegrationServicesProjects) o con un instalador independiente que se puede descargar [aquí](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt?view=sql-server-2017#ssdt-for-vs-2017-standalone-installer), puede encontrar la nueva propiedad **ConnectByProxy** que se ha agregado en los administradores de conexiones de OLEDB o de archivo plano.  

Al diseñar nuevos paquetes que contienen tareas Flujo de datos con orígenes de OLEDB o de archivo plano para tener acceso a bases de datos o archivos locales, puede habilitar esta propiedad mediante su establecimiento en **True** en el panel Propiedades de los administradores de conexiones correspondientes.

![Habilitación de la propiedad ConnectByProxy](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-manager-properties.png)

También puede habilitar esta propiedad al ejecutar paquetes existentes sin tener que cambiarlos manualmente uno a uno.  Hay dos opciones:
- Abrir, recompilar y volver a implementar el proyecto que contiene los paquetes con el SSDT más reciente para ejecutarse en Azure-SSIS IR: La propiedad se puede habilitar después mediante el establecimiento en **True** para los administradores de conexiones correspondientes que aparecen en la pestaña **Administradores de conexiones** (Connection Managers) de la ventana emergente Execute Package (Ejecutar paquete) al ejecutar paquetes de SSMS.

  ![Habilitación de la propiedad ConnectByProxy 2](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-managers-tab-ssms.png)

  La propiedad también se puede habilitar mediante el establecimiento en **True** para los administradores de conexiones correspondientes que aparecen en la pestaña **Administradores de conexiones** (Connection Managers) de la actividad [Execute SSIS Package](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) (Ejecutar paquete de SSIS) al ejecutar paquetes en las canalizaciones de ADF.
  
  ![Habilitación de la propiedad ConnectByProxy 3](media/self-hosted-integration-runtime-proxy-ssis/shir-connection-managers-tab-ssis-activity.png)

- Volver a implementar el proyecto que contiene los paquetes que se van a ejecutar en el entorno de ejecución de integración de SSIS: La propiedad se puede habilitar después al proporcionar su ruta de acceso de propiedad, `\Package.Connections[YourConnectionManagerName].Properties[ConnectByProxy]`, y al establecerla en **True** como un reemplazo de la propiedad en la pestaña **Advanced** (Avanzadas) de la ventana emergente Execute Package (Ejecutar paquete) al ejecutar paquetes de SSMS.

  ![Habilitación de la propiedad ConnectByProxy 4](media/self-hosted-integration-runtime-proxy-ssis/shir-advanced-tab-ssms.png)

  La propiedad también se puede habilitar al proporcionar su ruta de acceso de propiedad, `\Package.Connections[YourConnectionManagerName].Properties[ConnectByProxy]`, y al establecerla en **True** como un reemplazo de la propiedad en la pestaña **Property Overrides** (Reemplazos de propiedad) de la actividad [Execute SSIS Package](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity) (Ejecutar paquete de SSIS) al ejecutar paquetes en las canalizaciones de ADF.
  
  ![Habilitación de la propiedad ConnectByProxy 5](media/self-hosted-integration-runtime-proxy-ssis/shir-property-overrides-tab-ssis-activity.png)

## <a name="debug-the-first-and-second-staging-tasks"></a>Depuración de la primera y segunda tareas de almacenamiento provisional

En la instancia del entorno de ejecución de integración autohospedado, puede encontrar los registros del entorno de ejecución en la carpeta `C:\ProgramData\SSISTelemetry` y los registros de ejecución de las primeras tareas de almacenamiento provisional en la carpeta `C:\ProgramData\SSISTelemetry\ExecutionLog`.  Los registros de ejecución de las segundas tareas de almacenamiento provisional se pueden encontrar en la instancia de SSISDB o en las rutas de acceso de registro especificadas, en función de si se almacenan los paquetes en SSISDB o en el sistema de archivo, recursos compartidos de archivos o Azure Files, respectivamente.  Los identificadores únicos de las primeras tareas de almacenamiento provisional también se pueden encontrar, por ejemplo, en los registros de ejecución de las segundas tareas de almacenamiento provisional. 

![Identificador único de la primera tarea de almacenamiento provisional](media/self-hosted-integration-runtime-proxy-ssis/shir-first-staging-task-guid.png)

## <a name="using-windows-authentication-in-staging-tasks"></a>Uso de la autenticación de Windows en tareas de almacenamiento provisional

Si las tareas de almacenamiento provisional del IR autohospedado requieren la autenticación de Windows, tiene que [configurar los paquetes SSIS para que usen la misma autenticación de Windows](https://docs.microsoft.com/sql/integration-services/lift-shift/ssis-azure-connect-with-windows-auth?view=sql-server-ver15). Las tareas de almacenamiento provisional se invocarán con la cuenta de servicio del IR autohospedado (`NT SERVICE\DIAHostService` de forma predeterminada) y se tendrá acceso a los almacenes de datos con la cuenta de autenticación de Windows. Ambas cuentas requieren que se les asignen determinadas directivas de seguridad. Por consiguiente, en la máquina del IR autohospedado, abra `Local Security Policy` -> `Local Policies` -> `User Rights Assignment` y complete los siguientes pasos.
- Asigne las directivas **Ajustar las cuotas de la memoria para un proceso** y **Reemplazar un símbolo (token) de nivel de proceso** a la cuenta de servicio del IR autohospedado. Esto se debe hacer automáticamente al instalar el IR autohospedado con la cuenta de servicio predeterminada. Si utiliza una cuenta de servicio diferente, asígnele las mismas directivas.
- Asigne la directiva **Iniciar sesión como servicio** a la cuenta de autenticación de Windows.

## <a name="billing-for-the-first-and-second-staging-tasks"></a>Facturación de la primera y segunda tareas de almacenamiento provisional

Las primeras tareas de almacenamiento provisional que se ejecutan en el entorno de ejecución de integración autohospedado se facturarán por separado de la misma manera que se facturan las actividades de movimiento de datos que se ejecutan en el entorno de ejecución de integración autohospedado según lo especificado en el artículo sobre [precios de la canalización de datos de ADF](https://azure.microsoft.com/pricing/details/data-factory/data-pipeline/).

Las segundas tareas de almacenamiento provisional que se ejecutan en Azure-SSIS IR no se facturarán por separado, pero la instancia de Azure-SSIS IR en ejecución se facturará tal como se especifica en el artículo sobre [precios de Azure-SSIS IR](https://azure.microsoft.com/pricing/details/data-factory/ssis/).

## <a name="current-limitations"></a>Limitaciones actuales

- Actualmente solo se admiten tareas de flujo de datos con administradores de conexiones de ODBC, OLEDB o archivos planos y orígenes de ODBC, OLEDB y archivos planos. 
- Solo se admiten actualmente los servicios vinculados de Azure Blob Storage configurados con la autenticación de **clave de cuenta**/**URI SAS**/**entidad de servicio**.

## <a name="next-steps"></a>Pasos siguientes

Cuando configure el entorno de ejecución de integración autohospedado como un proxy para Azure-SSIS IR, puede implementar y ejecutar los paquetes para acceder a los datos locales como actividades de ejecución de paquetes SSIS en las canalizaciones de ADF; consulte [Ejecución de un paquete de SSIS mediante la actividad de ejecución de paquete de SSIS en las canalizaciones de Azure Data Factory](https://docs.microsoft.com/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).