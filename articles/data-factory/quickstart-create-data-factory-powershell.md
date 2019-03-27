---
title: Copia de datos en Blob Storage mediante Azure Data Factory | Microsoft Docs
description: Cree una factoría de datos de Azure para copiar los datos de una ubicación de Azure Blob Storage a otra.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: quickstart
ms.date: 01/22/2018
ms.author: jingwang
ms.openlocfilehash: b675ab9663be674ec2439bfe9139b7c79c144cbd
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57453395"
---
# <a name="quickstart-create-an-azure-data-factory-using-powershell"></a>Inicio rápido: Creación de una factoría de datos de Azure con PowerShell

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versión 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Versión actual](quickstart-create-data-factory-powershell.md)

En esta guía de inicio rápido se describe cómo usar PowerShell para crear una instancia de Azure Data Factory. La canalización que ha creado en esta factoría de datos **copia** los datos de una carpeta a otra en Azure Blob Storage. Para ver un tutorial acerca de cómo **transformar** datos mediante Azure Data Factory, consulte [Tutorial: Transformación de datos con Spark](transform-data-using-spark.md).

> [!NOTE]
> En este artículo no se ofrece una introducción detallada al servicio Data Factory. Para ver una introducción al servicio Azure Data Factory, consulte [Introducción al servicio Azure Data Factory](introduction.md).

[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)]

### <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Instale los módulos de Azure PowerShell siguiendo las instrucciones de [Cómo instalar y configurar Azure PowerShell](/powershell/azure/install-Az-ps).

#### <a name="log-in-to-powershell"></a>Inicio de sesión en PowerShell

1. Inicie **PowerShell** en su equipo. Mantenga PowerShell abierto hasta el final de esta guía de inicio rápido. Si lo cierra y vuelve a abrirlo, deberá ejecutar los comandos de nuevo.

2. Ejecute el siguiente comando y escriba los mismos nombre de usuario y contraseña de Azure que utiliza para iniciar sesión en Azure Portal:

    ```powershell
    Connect-AzAccount
    ```

3. Ejecute el siguiente comando para ver todas las suscripciones de esta cuenta:

    ```powershell
    Get-AzSubscription
    ```

4. Si ve varias suscripciones de Azure asociadas a su cuenta, ejecute el siguiente comando para seleccionar la suscripción con la que desea trabajar. Reemplace **SubscriptionId** con el identificador de la suscripción de Azure:

    ```powershell
    Select-AzSubscription -SubscriptionId "<SubscriptionId>"
    ```

## <a name="create-a-data-factory"></a>Crear una factoría de datos

1. Defina una variable para el nombre del grupo de recursos que usa en los comandos de PowerShell más adelante. Copie el texto del comando siguiente en PowerShell, especifique el nombre del [grupo de recursos de Azure](../azure-resource-manager/resource-group-overview.md) entre comillas dobles y ejecute el comando. Por ejemplo: `"ADFQuickStartRG"`.

     ```powershell
    $resourceGroupName = "ADFQuickStartRG";
    ```

    Si el grupo de recursos ya existe, puede que no desee sobrescribirlo. Asigne otro valor a la variable `$ResourceGroupName` y vuelva a ejecutar el comando

2. Para crear el grupo de recursos de Azure, ejecute el comando siguiente:

    ```powershell
    $ResGrp = New-AzResourceGroup $resourceGroupName -location 'East US'
    ```

    Si el grupo de recursos ya existe, puede que no desee sobrescribirlo. Asigne otro valor a la variable `$ResourceGroupName` y ejecute el comando de nuevo.

3. Defina una variable para el nombre de la factoría de datos. 

    > [!IMPORTANT]
    >  Actualice el nombre de la factoría de datos para que sea globalmente único. Por ejemplo, ADFTutorialFactorySP1127.

    ```powershell
    $dataFactoryName = "ADFQuickStartFactory";
    ```

4. Para crear la factoría de datos, ejecute el siguiente cmdlet **Set-AzDataFactoryV2** con las propiedades ResourceGroupName y Location de la variable $ResGrp:

    ```powershell
    $DataFactory = Set-AzDataFactoryV2 -ResourceGroupName $ResGrp.ResourceGroupName `
        -Location $ResGrp.Location -Name $dataFactoryName
    ```

Tenga en cuenta los siguientes puntos:

* El nombre de la instancia de Azure Data Factory debe ser único de forma global. Si recibe el siguiente error, cambie el nombre y vuelva a intentarlo.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```

* Para crear instancias de Data Factory, la cuenta de usuario que use para iniciar sesión en Azure debe ser un miembro de los roles **colaborador** o **propietario**, o de **administrador** de la suscripción de Azure.

* Para una lista de las regiones de Azure en las que Data Factory está disponible actualmente, seleccione las regiones que le interesen en la página siguiente y expanda **Análisis** para poder encontrar **Data Factory**: [Productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/). Los almacenes de datos (Azure Storage, Azure SQL Database, etc.) y los procesos (HDInsight, etc.) que usa la factoría de datos pueden encontrarse en otras regiones.

## <a name="create-a-linked-service"></a>Creación de un servicio vinculado

Cree servicios vinculados en una factoría de datos para vincular los almacenes de datos y los servicios de proceso a la factoría de datos. En esta guía de inicio rápido, creará un servicio vinculado de Azure Storage que se utiliza para ambos almacenes: de origen y de recepción. El servicio vinculado tiene la información de conexión que usa el servicio Data Factory en el entorno de tiempo de ejecución para conectarse a él.

1. Cree un archivo JSON con el nombre **AzureStorageLinkedService.json** en la carpeta **C:\ADFv2QuickStartPSH** con el siguiente contenido: (Cree la carpeta ADFv2QuickStartPSH si todavía no existe).

    > [!IMPORTANT]
    > Reemplace &lt;accountName&gt; y &lt;accountKey&gt; por el nombre y la clave de su cuenta de Azure Storage antes de guardar el archivo.

    ```json
    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": {
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountName>;AccountKey=<accountKey>;EndpointSuffix=core.windows.net",
                    "type": "SecureString"
                }
            }
        }
    }
    ```

    Si usa el Bloc de notas, seleccione **Todos los archivos** en el campo **Guardar como tipo** del cuadro de diálogo **Guardar como**. Si no lo hace, puede que se agregue la extensión `.txt` al archivo. Por ejemplo, `AzureStorageLinkedService.json.txt`. Si crea el archivo en el Explorador de archivos antes de abrirlo en el Bloc de notas, puede que no vea la extensión `.txt`, ya que la opción **Ocultar las extensiones de archivo para tipos de archivo conocidos** está establecida de forma predeterminada. Quite la extensión `.txt` antes de continuar con el paso siguiente.

2. En **PowerShell**, cambie a la carpeta **ADFv2QuickStartPSH**.

    ```powershell
    Set-Location 'C:\ADFv2QuickStartPSH'
    ```

3. Ejecute el cmdlet **Set-AzDataFactoryV2LinkedService** para crear el servicio vinculado: **AzureStorageLinkedService**.

    ```powershell
    Set-AzDataFactoryV2LinkedService -DataFactoryName $DataFactory.DataFactoryName `
        -ResourceGroupName $ResGrp.ResourceGroupName -Name "AzureStorageLinkedService" `
        -DefinitionFile ".\AzureStorageLinkedService.json"
    ```

    Este es la salida de ejemplo:

    ```console
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

## <a name="create-a-dataset"></a>Crear un conjunto de datos

En este paso, define un conjunto de datos que representa los datos que se copian de un origen a un receptor. El conjunto de datos es de tipo **AzureBlob**. Hace referencia al **servicio vinculado de Azure Storage** que creó en el paso anterior. Toma un parámetro para construir la propiedad **folderPath**. Para un conjunto de datos de entrada, la actividad de copia de la canalización pasa la ruta de acceso de entrada como un valor para este parámetro. De forma similar, para un conjunto de datos de salida, la actividad de copia pasa la ruta de acceso de salida como un valor para este parámetro. 

1. Cree un archivo JSON llamado **BlobDataset.json** en la carpeta **C:\ADFv2QuickStartPSH** con el siguiente contenido:

    ```json
    {
        "name": "BlobDataset",
        "properties": {
            "type": "AzureBlob",
            "typeProperties": {
                "folderPath": "@{dataset().path}"
            },
            "linkedServiceName": {
                "referenceName": "AzureStorageLinkedService",
                "type": "LinkedServiceReference"
            },
            "parameters": {
                "path": {
                    "type": "String"
                }
            }
        }
    }
    ```

2. Para crear el conjunto de datos: **BlobDataset**, ejecute el cmdlet **Set-AzDataFactoryV2Dataset**.

    ```powershell
    Set-AzDataFactoryV2Dataset -DataFactoryName $DataFactory.DataFactoryName `
        -ResourceGroupName $ResGrp.ResourceGroupName -Name "BlobDataset" `
        -DefinitionFile ".\BlobDataset.json"
    ```

    Este es la salida de ejemplo:

    ```console
    DatasetName       : BlobDataset
    ResourceGroupName : <resourceGroupname>
    DataFactoryName   : <dataFactoryName>
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureBlobDataset
    ```

## <a name="create-a-pipeline"></a>Crear una canalización

En esta guía de inicio rápido, creará una canalización con una actividad que admita dos parámetros: ruta de acceso de blob de entrada y de salida. Los valores para estos parámetros se establecen cuando se desencadena/ejecuta la canalización. La actividad de copia usa el mismo conjunto de datos de blob creado en el paso anterior como entrada y salida. Cuando se usa el conjunto de datos como conjunto de datos de entrada, se especifica una ruta de acceso de entrada. Cuando se usa el conjunto de datos como conjunto de datos de salida, se especifica una ruta de acceso de salida.

1. Cree un archivo JSON llamado **Adfv2QuickStartPipeline.json** en la carpeta **C:\ADFv2QuickStartPSH** con el siguiente contenido:

    ```json
    {
        "name": "Adfv2QuickStartPipeline",
        "properties": {
            "activities": [
                {
                    "name": "CopyFromBlobToBlob",
                    "type": "Copy",
                    "inputs": [
                        {
                            "referenceName": "BlobDataset",
                            "parameters": {
                                "path": "@pipeline().parameters.inputPath"
                            },
                            "type": "DatasetReference"
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "BlobDataset",
                            "parameters": {
                                "path": "@pipeline().parameters.outputPath"
                            },
                            "type": "DatasetReference"
                        }
                    ],
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    }
                }
            ],
            "parameters": {
                "inputPath": {
                    "type": "String"
                },
                "outputPath": {
                    "type": "String"
                }
            }
        }
    }
    ```

2. Para crear la canalización: **Adfv2QuickStartPipeline**, ejecute el cmdlet **Set-AzDataFactoryV2Pipeline**.

    ```powershell
    $DFPipeLine = Set-AzDataFactoryV2Pipeline `
        -DataFactoryName $DataFactory.DataFactoryName `
        -ResourceGroupName $ResGrp.ResourceGroupName `
        -Name "Adfv2QuickStartPipeline" `
        -DefinitionFile ".\Adfv2QuickStartPipeline.json"
    ```

## <a name="create-a-pipeline-run"></a>Creación de una ejecución de canalización

En este paso, define los valores para los parámetros de canalización: **inputPath** y **outputPath** con valores reales de rutas de acceso de blob de origen y receptor. A continuación, use estos argumentos para crear una canalización.

1. Cree un archivo JSON llamado **BlobDataset.json** en la carpeta **C:\ADFv2QuickStartPSH** con el siguiente contenido:

    ```json
    {
        "inputPath": "adftutorial/input",
        "outputPath": "adftutorial/output"
    }
    ```
2. Ejecute el cmdlet **Invoke-AzDataFactoryV2Pipeline** para crear una ejecución de canalización y pasar los valores de parámetro. El cmdlet devuelve el identificador de ejecución de la canalización para realizar una supervisión en un futuro.

    ```powershell
    $RunId = Invoke-AzDataFactoryV2Pipeline `
        -DataFactoryName $DataFactory.DataFactoryName `
        -ResourceGroupName $ResGrp.ResourceGroupName `
        -PipelineName $DFPipeLine.Name `
        -ParameterFile .\PipelineParameters.json
    ```

## <a name="monitor-the-pipeline-run"></a>Supervisión de la ejecución de la canalización

1. Ejecute el script de PowerShell siguiente para comprobar continuamente el estado de ejecución de la canalización hasta que termine de copiar los datos. Copie y pegue el siguiente script en la ventana de PowerShell y presione ENTRAR.

    ```powershell
    while ($True) {
        $Run = Get-AzDataFactoryV2PipelineRun `
            -ResourceGroupName $ResGrp.ResourceGroupName `
            -DataFactoryName $DataFactory.DataFactoryName `
            -PipelineRunId $RunId

        if ($Run) {
            if ($run.Status -ne 'InProgress') {
                Write-Output ("Pipeline run finished. The status is: " +  $Run.Status)
                $Run
                break
            }
            Write-Output "Pipeline is running...status: InProgress"
        }

        Start-Sleep -Seconds 10
    }
    ```

    Esta es la salida de ejemplo de la ejecución de canalización:

    ```console
    Pipeline is running...status: InProgress
    Pipeline run finished. The status is:  Succeeded
    
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : SPTestFactory0928
    RunId             : 0000000000-0000-0000-0000-0000000000000
    PipelineName      : Adfv2QuickStartPipeline
    LastUpdated       : 9/28/2017 8:28:38 PM
    Parameters        : {[inputPath, adftutorial/input], [outputPath, adftutorial/output]}
    RunStart          : 9/28/2017 8:28:14 PM
    RunEnd            : 9/28/2017 8:28:38 PM
    DurationInMs      : 24151
    Status            : Succeeded
    Message           :
    ```

    Puede aparecer el error siguiente:

    ```console
    Activity CopyFromBlobToBlob failed: Failed to detect region of linked service 'AzureStorage' : 'AzureStorageLinkedService' with error '[Region Resolver] Azure Storage failed to get address for DNS. Warning: System.Net.Sockets.SocketException (0x80004005): No such host is known
    ```

    Si ve el error, realice los pasos siguientes:

    1. En el archivo AzureStorageLinkedService.json, confirme que el nombre y la clave de la cuenta de Azure Storage son correctos.
    2. Compruebe que el formato de la cadena de conexión es correcto. Las propiedades, por ejemplo, AccountName y AccountKey, están separadas por el carácter de punto y coma (`;`).
    3. Si el nombre y la clave de la cuenta están entre corchetes, quítelos.
    4. A continuación, se muestra una cadena de conexión de ejemplo:

        ```json
        "connectionString": {
            "value": "DefaultEndpointsProtocol=https;AccountName=mystorageaccountname;AccountKey=mystorageaccountkey;EndpointSuffix=core.windows.net",
            "type": "SecureString"
        }
        ```

    5. Vuelva a crear el servicio vinculado siguiendo los pasos de la sección [Creación de un servicio vinculado](#create-a-linked-service).
    6. Vuelva a ejecutar la canalización siguiendo los pasos de la sección [Creación de una ejecución de canalización ](#create-a-pipeline-run).
    7. Ejecute el comando de supervisión actual para supervisar la nueva ejecución de canalización.

2. Ejecute el script siguiente para recuperar detalles de la ejecución de la actividad de copia, como el tamaño de los datos leídos o escritos.

    ```powershell
    Write-Output "Activity run details:"
    $Result = Get-AzDataFactoryV2ActivityRun -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -PipelineRunId $RunId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)
    $Result

    Write-Output "Activity 'Output' section:"
    $Result.Output -join "`r`n"

    Write-Output "Activity 'Error' section:"
    $Result.Error -join "`r`n"
    ```
3. Confirme que ha obtenido una salida similar a la siguiente salida de ejemplo de resultado de ejecución de actividad:

    ```console
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : SPTestFactory0928
    ActivityName      : CopyFromBlobToBlob
    PipelineRunId     : 00000000000-0000-0000-0000-000000000000
    PipelineName      : Adfv2QuickStartPipeline
    Input             : {source, sink}
    Output            : {dataRead, dataWritten, copyDuration, throughput...}
    LinkedServiceName :
    ActivityRunStart  : 9/28/2017 8:28:18 PM
    ActivityRunEnd    : 9/28/2017 8:28:36 PM
    DurationInMs      : 18095
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    Activity 'Output' section:
    "dataRead": 38
    "dataWritten": 38
    "copyDuration": 7
    "throughput": 0.01
    "errors": []
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (West US)"
    "usedDataIntegrationUnits": 2
    "billedDuration": 14
    ```

[!INCLUDE [data-factory-quickstart-verify-output-cleanup.md](../../includes/data-factory-quickstart-verify-output-cleanup.md)]

## <a name="next-steps"></a>Pasos siguientes

La canalización de este ejemplo copia los datos de una ubicación a otra en una instancia de Azure Blob Storage. Consulte los [tutoriales](tutorial-copy-data-dot-net.md) para obtener información acerca del uso de Data Factory en otros escenarios.
