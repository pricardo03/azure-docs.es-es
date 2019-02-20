---
title: 'Ejecución de paquetes SSIS con la actividad Ejecutar paquete de SSIS: Azure | Microsoft Docs'
description: En este artículo se describe cómo ejecutar un paquete de SQL Server Integration Services (SSIS) desde una canalización de Azure Data Factory mediante la actividad Ejecutar paquete de SSIS.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 02/12/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 2a948a75ce3f6c21d7e92e3e1ccb1ef98dbe2ea0
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56114390"
---
# <a name="run-an-ssis-package-with-the-execute-ssis-package-activity-in-azure-data-factory"></a>Ejecución de un paquete de SSIS mediante la actividad Ejecutar paquete SSIS de Azure Data Factory
En este artículo se describe cómo ejecutar un paquete SSIS desde una canalización de Azure Data Factory mediante la actividad Ejecutar paquete de SSIS. 

## <a name="prerequisites"></a>Requisitos previos

Cree una instancia de Azure-SSIS Integration Runtime (IR) si no tiene ya una. Para ello, siga las instrucciones paso a paso del [Tutorial: Implementación de paquetes SSIS en Azure](tutorial-create-azure-ssis-runtime-portal.md).

## <a name="run-a-package-in-the-azure-portal"></a>Ejecución de un paquete en Azure Portal
En esta sección, usará la interfaz de usuario (UI) o aplicación de ADF para crear una canalización de ADF con la actividad Ejecutar paquete de SSIS que ejecuta el paquete de SSIS.

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>Creación de una canalización con una actividad Ejecutar paquete de SSIS
En este paso, usará la interfaz de usuario o aplicación de ADF para crear una canalización. Agregará una actividad Ejecutar paquete de SSIS a la canalización y la configurará para ejecutar el paquete de SSIS. 

1. En la página de inicio o información general de ADF de Azure Portal, haga clic en el icono **Author & Monitor** (Creación y supervisión) para iniciar la interfaz de usuario o aplicación de ADF en una pestaña aparte. 

   ![Página principal Factoría de datos](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)

   En la página **Let's get started** (Empecemos), haga clic en **Create pipeline** (Crear canalización): 

   ![Página de introducción](./media/how-to-invoke-ssis-package-stored-procedure-activity/get-started-page.png)

2. En el cuadro de herramientas **Activities** (Actividades), expanda **General**, arrastre la actividad **Execute SSIS Package** (Ejecutar paquete de SSIS) y colóquela en la superficie del diseñador de canalizaciones. 

   ![Arrastrar la actividad Execute SSIS Package (Ejecutar paquete de SSIS) a la superficie del diseñador](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-designer.png) 

3. En la pestaña **General** de la actividad Execute SSIS Package (Ejecutar paquete de SSIS), proporcione un nombre y una descripción para la actividad. Establezca el tiempo de espera opcional y los valores de reintento.

   ![Establecimiento de propiedades en la pestaña General](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-general.png)

4. En la pestaña **Settings** (Configuración) de la actividad Execute SSIS Package (Ejecutar paquete de SSIS), seleccione su instancia de Azure-SSIS IR que está asociada a la base de datos SSISDB donde está implementado el paquete. Si el paquete necesita el entorno de ejecución de 32 bits para funcionar, active la casilla **32-Bit runtime** (Entorno de ejecución de 32 bits). En **Logging level** (Nivel de registro), seleccione un ámbito predefinido de registro para la ejecución de su paquete. Active la casilla **Customized** (Personalizado), si quiere escribir en su lugar un nombre de registro personalizado. Cuando se ejecuta la instancia de Azure-SSIS IR y la casilla **Manual entries** (Entradas manuales) está desactivada, puede examinar y seleccionar sus carpetas, proyectos, paquetes y entornos existentes de SSISDB. Haga clic en el botón **Refresh** (Actualizar) para capturar las carpetas, proyectos, paquetes y entornos recién agregados de SSISDB, de forma que estén disponibles para su examen y selección. 

   ![Establecer propiedades de la pestaña Settings (Configuración): Automatic (Automática)](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings.png)

   Cuando la instancia de Azure-SSIS IR no se está ejecutando o la casilla **Manual entries** (Entradas manuales) está activada, puede escribir las rutas de acceso de su paquete y entorno de SSISDB en los siguientes formatos: `<folder name>/<project name>/<package name>.dtsx` y `<folder name>/<environment name>`.

   ![Establecer las propiedades en la pestaña Settings (Configuración): manual](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-settings2.png)

5. En la pestaña **SSIS Parameters** (Parámetros de SSIS) de la actividad Execute SSIS Package (Ejecutar paquete de SSIS), cuando la instancia de Azure-SSIS IR está en ejecución y la casilla **Manual entries** (Entradas manuales) de la pestaña **Settings** (Configuración) está desactivada, se muestran los parámetros de SSIS existentes en el proyecto o paquete seleccionados de SSISDB para que les asigne valores. En caso contrario, puede escribirlos uno a uno para asignar valores manualmente. Asegúrese de que existen y se han escrito correctamente para que la ejecución del paquete se realice de forma adecuada. También puede agregar contenido dinámico a sus valores mediante expresiones, funciones, variables del sistema de ADF y parámetros o variables de canalización de ADF.

   ![Establecer propiedades en la pestaña SSIS Parameters (Parámetros de SSIS)](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-ssis-parameters.png)

6. En la pestaña **Connection Managers** (Administradores de conexiones) de la actividad Execute SSIS Package (Ejecutar paquete de SSIS), cuando la instancia de Azure-SSIS IR está en ejecución y la casilla **Manual entries** (Entradas manuales) de la pestaña **Settings** (Configuración) está desactivada, se muestran los administradores de conexiones en el proyecto o paquete seleccionados de SSISDB para que les asigne valores. En caso contrario, puede escribirlos uno a uno para asignar valores manualmente. Asegúrese de que existen y se han escrito correctamente para que la ejecución del paquete se realice de forma adecuada. También puede agregar contenido dinámico a sus valores mediante expresiones, funciones, variables del sistema de ADF y parámetros o variables de canalización de ADF.

   ![Establecer propiedades en la pestaña Connection Managers (Administradores de conexiones)](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-connection-managers.png)

7. En la pestaña **Property Overrides** (Reemplazos de propiedad) de la actividad Execute SSIS Package (Ejecutar paquete de SSIS), puede escribir las rutas de acceso de las propiedades existentes en el paquete seleccionado de SSISDB una a una para asignarles valores manualmente. Asegúrese de que existen y de que se escriben correctamente para que la ejecución del paquete se realice de forma adecuada; por ejemplo, para invalidar el valor de la variable de usuario, escriba su ruta de acceso en el siguiente formato: `\Package.Variables[User::YourVariableName].Value`. También puede agregar contenido dinámico a sus valores mediante expresiones, funciones, variables del sistema de ADF y parámetros o variables de canalización de ADF.

   ![Establecer propiedades en la pestaña Property Overrides (Reemplazos de propiedad)](media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-property-overrides.png)

8. Para validar la configuración de la canalización, haga clic en **Validate** (Validar) en la barra de herramientas. Para cerrar **Pipeline Validation Report** (Informe de comprobación de la canalización), haga clic en **>>**.

9. Para publicar la canalización en ADF, haga clic en **Publish All** (Publicar todo). 

### <a name="run-the-pipeline"></a>Ejecución de la canalización
En este paso, desencadenará una ejecución de canalización. 

1. Para desencadenar una ejecución de canalización, haga clic en **Trigger** (Desencadenar) en la barra de herramientas y en **Trigger now** (Desencadenar ahora). 

   ![Trigger now (Desencadenar ahora)](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-trigger.png)

2. En la ventana **Pipeline Run** (Ejecución de canalización), seleccione **Finish** (Finalizar). 

### <a name="monitor-the-pipeline"></a>Supervisar la canalización

1. Cambie a la pestaña **Monitor** (Supervisar) de la izquierda. Verá la ejecución de canalización y su estado junto con otro tipo de información (como la hora de inicio de la ejecución). Para actualizar la vista, haga clic en **Refresh** (Actualizar).

   ![Ejecuciones de la canalización](./media/how-to-invoke-ssis-package-stored-procedure-activity/pipeline-runs.png)

2. Haga clic en el vínculo **View Activity Runs** (Ver ejecuciones de actividad) de la columna **Actions** (Acciones). Solo verá una ejecución de actividad porque la canalización solo tiene una actividad (actividad Ejecutar paquete de SSIS).

   ![Ejecuciones de actividad](./media/how-to-invoke-ssis-package-ssis-activity/ssis-activity-runs.png)

3. Puede ejecutar la **consulta** siguiente en la base de datos SSISDB en el servidor de Azure SQL para comprobar la ejecución del paquete. 

   ```sql
   select * from catalog.executions
   ```

   ![Comprobación de las ejecuciones del paquete](./media/how-to-invoke-ssis-package-stored-procedure-activity/verify-package-executions.png)

4. También puede obtener el identificador de ejecución de SSISDB desde la salida de la ejecución de la actividad de canalización, y usar el identificador para comprobar registros de ejecución y mensajes de error más completos en SSMS.

   ![Obtenga el identificador de ejecución.](media/how-to-invoke-ssis-package-ssis-activity/get-execution-id.png)

### <a name="schedule-the-pipeline-with-a-trigger"></a>Programación de la canalización con un desencadenador

También puede crear un desencadenador programado para la canalización de manera que esta se ejecute según una programación (por hora, cada día, etc.). Para ver un ejemplo, consulte [Create a data factory - Data Factory UI](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule) (Creación de una factoría de datos: interfaz de usuario de Data Factory).

## <a name="run-a-package-with-powershell"></a>Ejecución de un paquete con PowerShell
En esta sección, usará Azure PowerShell para crear una canalización de ADF con una actividad Ejecutar paquete de SSIS que ejecuta el paquete de SSIS. 

Instale los módulos de Azure PowerShell más recientes siguiendo las instrucciones paso a paso que se indican en [Cómo instalar y configurar Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps).

### <a name="create-an-adf-with-azure-ssis-ir"></a>Creación de un ADF con Azure-SSIS IR
Puede usar un ADF existente que ya tenga aprovisionado Azure-SSIS IR o crear un ADF con Azure-SSIS IR siguiendo las instrucciones paso a paso del [Tutorial: Implementación de paquetes SSIS en Azure mediante PowerShell](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure-powershell).

### <a name="create-a-pipeline-with-an-execute-ssis-package-activity"></a>Creación de una canalización con una actividad Ejecutar paquete de SSIS 
En este paso se crea una canalización con una actividad Ejecutar paquete de SSIS. La actividad ejecuta el paquete de SSIS. 

1. Cree un archivo JSON con el nombre **RunSSISPackagePipeline.json** en la carpeta **C:\ADF\RunSSISPackage** con un contenido similar al del siguiente ejemplo:

   > [!IMPORTANT]
   > Reemplace los nombres de objeto, descripciones, rutas de acceso, valores de propiedades y parámetros, contraseñas y otros valores de variables antes de guardar el archivo. 

   ```json
   {
       "name": "RunSSISPackagePipeline",
       "properties": {
           "activities": [{
               "name": "mySSISActivity",
               "description": "My SSIS package/activity description",
               "type": "ExecuteSSISPackage",
               "typeProperties": {
                   "connectVia": {
                       "referenceName": "myAzureSSISIR",
                       "type": "IntegrationRuntimeReference"
                   },
                   "runtime": "x64",
                   "loggingLevel": "Basic",
                   "packageLocation": {
                       "packagePath": "FolderName/ProjectName/PackageName.dtsx"
                   },
                   "environmentPath": "FolderName/EnvironmentName",
                   "projectParameters": {
                       "project_param_1": {
                           "value": "123"
                       }
                   },
                   "packageParameters": {
                       "package_param_1": {
                           "value": "345"
                       }
                   },
                   "projectConnectionManagers": {
                       "MyAdonetCM": {
                           "userName": {
                               "value": "sa"
                           },
                           "passWord": {
                               "value": {
                                   "type": "SecureString",
                                   "value": "abc"
                               }
                           }
                       }
                   },
                   "packageConnectionManagers": {
                       "MyOledbCM": {
                           "userName": {
                               "value": "sa"
                           },
                           "passWord": {
                               "value": {
                                   "type": "SecureString",
                                   "value": "def"
                               }
                           }
                       }
                   },
                   "propertyOverrides": {
                       "\\Package.MaxConcurrentExecutables": {
                           "value": 8,
                           "isSensitive": false
                       }
                   }
               },
               "policy": {
                   "timeout": "0.01:00:00",
                   "retry": 0,
                   "retryIntervalInSeconds": 30
               }
           }]
       }
   }
   ```

2. En Azure PowerShell, cambie a la carpeta `C:\ADF\RunSSISPackage`.

3. Para crear la canalización **RunSSISPackagePipeline**, ejecute el cmdlet **Set-AzureRmDataFactoryV2Pipeline**.

   ```powershell
   $DFPipeLine = Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                                  -ResourceGroupName $ResGrp.ResourceGroupName `
                                                  -Name "RunSSISPackagePipeline"
                                                  -DefinitionFile ".\RunSSISPackagePipeline.json"
   ```

   Este es la salida de ejemplo:

   ```
   PipelineName      : Adfv2QuickStartPipeline
   ResourceGroupName : <resourceGroupName>
   DataFactoryName   : <dataFactoryName>
   Activities        : {CopyFromBlobToBlob}
   Parameters        : {[inputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification], [outputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
   ```

### <a name="run-the-pipeline"></a>Ejecución de la canalización
Use el cmdlet **Invoke-AzureRmDataFactoryV2Pipeline** para ejecutar la canalización. El cmdlet devuelve el identificador de ejecución de la canalización para realizar una supervisión en un futuro.

```powershell
$RunId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName `
                                             -ResourceGroupName $ResGrp.ResourceGroupName `
                                             -PipelineName $DFPipeLine.Name
```

### <a name="monitor-the-pipeline"></a>Supervisar la canalización

Ejecute el script de PowerShell siguiente para comprobar continuamente el estado de ejecución de la canalización hasta que termine de copiar los datos. Copie y pegue el siguiente script en la ventana de PowerShell y presione ENTRAR. 

```powershell
while ($True) {
    $Run = Get-AzureRmDataFactoryV2PipelineRun -ResourceGroupName $ResGrp.ResourceGroupName `
                                               -DataFactoryName $DataFactory.DataFactoryName `
                                               -PipelineRunId $RunId

    if ($Run) {
        if ($run.Status -ne 'InProgress') {
            Write-Output ("Pipeline run finished. The status is: " +  $Run.Status)
            $Run
            break
        }
        Write-Output  "Pipeline is running...status: InProgress"
    }

    Start-Sleep -Seconds 10
}   
```

También puede supervisar la canalización mediante Azure Portal. Para ver instrucciones paso a paso, consulte [Supervisar la canalización](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

### <a name="schedule-the-pipeline-with-a-trigger"></a>Programación de la canalización con un desencadenador
En el paso anterior, ejecutó la canalización a petición. También puede crear un desencadenador de programación para ejecutar la canalización en una programación (cada hora, día, etc.).

1. Cree un archivo JSON con el nombre **MyTrigger.json** en la carpeta **C:\ADF\RunSSISPackage** con el siguiente contenido: 

   ```json
   {
       "properties": {
           "name": "MyTrigger",
           "type": "ScheduleTrigger",
           "typeProperties": {
               "recurrence": {
                   "frequency": "Hour",
                   "interval": 1,
                   "startTime": "2017-12-07T00:00:00-08:00",
                   "endTime": "2017-12-08T00:00:00-08:00"
               }
           },
           "pipelines": [{
               "pipelineReference": {
                   "type": "PipelineReference",
                   "referenceName": "RunSSISPackagePipeline"
               },
               "parameters": {}
           }]
       }
   }    
   ```
2. En **Azure PowerShell**, cambie a la carpeta **C:\ADF\RunSSISPackage**.
3. Ejecute el cmdlet **Set-AzureRmDataFactoryV2Trigger** para crear el desencadenador. 

   ```powershell
   Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                   -DataFactoryName $DataFactory.DataFactoryName `
                                   -Name "MyTrigger" -DefinitionFile ".\MyTrigger.json"
   ```
4. De manera predeterminada, el desencadenador está en estado detenido. Para iniciar el desencadenador ejecute el cmdlet **Start-AzureRmDataFactoryV2Trigger**. 

   ```powershell
   Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName `
                                     -DataFactoryName $DataFactory.DataFactoryName `
                                     -Name "MyTrigger" 
   ```
5. Confirme que el desencadenador se haya iniciado con la ejecución del cmdlet **Get-AzureRmDataFactoryV2Trigger**. 

   ```powershell
   Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName `
                                   -DataFactoryName $DataFactoryName `
                                   -Name "MyTrigger"     
   ```    
6. Ejecute el comando siguiente al comenzar la hora siguiente. Por ejemplo, si la hora actual es 15:25 UTC, ejecute el comando a las 16:00 UTC. 
    
   ```powershell
   Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName `
                                      -DataFactoryName $DataFactoryName `
                                      -TriggerName "MyTrigger" `
                                      -TriggerRunStartedAfter "2017-12-06" `
                                      -TriggerRunStartedBefore "2017-12-09"
   ```

   Puede ejecutar la consulta siguiente en la base de datos SSISDB en el servidor de Azure SQL para comprobar la ejecución del paquete. 

   ```sql
   select * from catalog.executions
   ```

## <a name="next-steps"></a>Pasos siguientes
Vea la siguiente entrada de blog:
-   [Modernize and extend your ETL/ELT workflows with SSIS activities in ADF pipelines](https://blogs.msdn.microsoft.com/ssis/2018/05/23/modernize-and-extend-your-etlelt-workflows-with-ssis-activities-in-adf-pipelines/) (Modernización y ampliación de los flujos de trabajo ETL/ETL con actividades de SSIS en las canalizaciones de ADF)
