---
title: Almacenamiento de salidas y entradas de los trabajos de Batch AI con Azure Storage | Microsoft Docs
description: Cómo usar Azure Storage con Batch AI para almacenar en la nube los archivos de salida y entrada de forma fácil y rápida
services: batch-ai
documentationcenter: ''
author: kevwang1
manager: jeconnoc
editor: ''
ms.service: batch-ai
ms.topic: article
ms.date: 08/14/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 42697f7f4bb8c6b9ef785eef0fe2f5f33b2b38a7
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "51615610"
---
# <a name="store-batch-ai-job-input-and-output-with-azure-storage"></a>Almacenamiento de salidas y entradas de los trabajos de Batch AI con Azure Storage

En esta guía se describe cómo usar Azure Storage para almacenar los archivos de entrada y salida durante la ejecución de un trabajo. Azure Storage es una de muchas opciones de almacenamiento compatibles con Batch AI. Batch AI se integra con Azure Storage mediante el montaje de sistemas de Azure Storage a un sistema de archivos de trabajo o de clúster de Batch AI, lo que permite un acceso ininterrumpido a los archivos almacenados en la nube. 

## <a name="introduction-to-azure-storage"></a>Introducción a Azure Storage

Azure Storage es una solución de almacenamiento en la nube de Microsoft. Batch AI es compatible con el montaje de contenedores de blobs de Azure y recursos compartidos de archivo a trabajos o clústeres de Batch AI, lo que permite acceder a los archivos desde un trabajo como si estuvieran en el sistema de archivos nativo. Batch AI monta los contenedores de blobs de Azure con [blobfuse](https://github.com/Azure/azure-storage-fuse), y los recursos compartidos de archivo de Azure a través del protocolo SMB. Para obtener más información sobre Azure Storage, consulte [Introducción a Azure Storage](../storage/common/storage-introduction.md).

## <a name="store-datasets-and-input-scripts-in-azure-storage"></a>Almacenamiento de conjuntos de datos y scripts de entrada en Azure Storage

Si elige Azure Storage para su entorno de Batch AI, se recomienda que almacene los archivos de entrada (como los conjuntos de datos) en un contenedor de blobs, ya que tiene un mayor rendimiento, y que almacene la salida de entrenamiento en un recurso compartido de archivos, ya que es compatible con la transmisión por secuencias (lo que permite leer los registros de salida de forma simultánea a la ejecución del trabajo). 

Antes de usar Azure Storage, debe [crear una cuenta de Azure Storage](../storage/common/storage-quickstart-create-account.md). Batch AI admite el montaje de volúmenes desde cuentas de Azure Storage tipo uso general v1 (GPv1) y uso general v2 (GPv2). La cuenta de Azure Storage puede contener varios contenedores de blobs o instancias de recurso compartido de archivos. Al elegir qué tipo de cuenta de almacenamiento va a crear, tenga en cuenta los requisitos de costo y rendimiento. Para más información, consulte [Introducción a las cuentas de Azure Storage](../storage/common/storage-account-overview.md). 

Para crear un contenedor de blobs y cargar el conjunto de datos en un contenedor de blobs de Azure, elija uno de los métodos siguientes:
- [Azure Portal](../storage/blobs/storage-quickstart-blobs-portal.md) para realizar la carga con una interfaz gráfica de usuario basada en web. Para cargar un número pequeño de archivos, Azure Portal proporciona el control más sencillo.
- [CLI de Azure Storage](../storage/blobs/storage-quickstart-blobs-cli.md) para realizar la carga a través de la línea de comandos (admite la carga de directorios). Para cargar directorios de archivos, use `az storage blob upload-batch`.
- [Otros métodos](../storage/common/storage-moving-data.md), incluido el uso de los SDK de aplicación.

De igual modo, para crear un recurso compartido de archivos de Azure, elija uno de los métodos siguientes:
- [Azure Portal](../storage/files/storage-how-to-use-files-portal.md)
- [CLI de Azure Storage](../storage/files/storage-how-to-use-files-cli.md)
- [Otros métodos](../storage/common/storage-moving-data.md)

### <a name="auto-storage-with-batch-ai"></a>Almacenamiento automático con Batch AI

También puede crear una cuenta de Azure Storage con un recurso compartido de archivos de Azure y un contenedor de blobs (y montar automáticamente estos volúmenes a un clúster de Batch AI) si usa el parámetro `--use-auto-storage` con `az batchai cluster create`. Para más información, consulte [esta página](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#auto-storage-account).

## <a name="mount-azure-storage"></a>Montaje de Azure Storage 

### <a name="mount-volumes-to-a-job"></a>Montaje de volúmenes a un trabajo

Montar un volumen de Azure Storage permite que sea accesible a través del sistema de archivos creado para cada trabajo. De esta forma, un trabajo puede leer y escribir archivos sin problemas en el almacenamiento en nube como si fueran archivos locales.

Para montar un volumen de Azure Storage a un trabajo creado con la CLI de Azure, use la propiedad `mountVolumes` en el archivo `job.json` mientras ejecuta `az batchai job create`. Para ver un ejemplo, consulte la [receta de GPU Tensorflow distribuida](https://github.com/Azure/BatchAI/blob/master/recipes/TensorFlow/TensorFlow-GPU-Distributed/job.json). El esquema para `mountVolumes` es:
```json
{
    "mountVolumes": {
        "azureFileShares": [{
            "azureFileUrl": "https://<STORAGE_ACCOUNT_NAME>.file.core.windows.net/<FILE_SHARE_NAME>",
            "relativeMountPath": "<RELATIVE_MOUNT_PATH>"
        }],
        "azureBlobFileSystems": [{
            "accountName": "<STORAGE_ACCOUNT_NAME>",
            "containerName": "<BLOB_CONTAINER_NAME>",
            "relativeMountPath": "<RELATIVE_MOUNT_PATH>"
        }]
    }
}
```

`azureFileShares` y `azureBlobFileSystems` son matrices de objetos que representan los volúmenes de montaje. Descripciones de los marcadores de posición:

- < RELATIVE_MOUNT_PATH >: el volumen se montará en esta ruta de acceso. Por ejemplo, si `relativeMountPath` es `jobs`, el volumen se ubicará en `$AZ_BATCHAI_JOB_MOUNT_ROOT/jobs`).
- < STORAGE_ACCOUNT_NAME >: el nombre de la cuenta de Azure Storage que contiene el recurso compartido de archivos o el contenedor de blobs.
- < FILE_SHARE_NAME >: el nombre del recurso compartido de archivos.
- <BLOB_CONTAINER_NAME>: el nombre del contenedor de blobs.

Para montar los volúmenes de Azure Storage con el SDK de Azure Batch AI, establezca la propiedad `mount_volumes` (Python) o `MountVolumes` (C#, Java) en `JobCreateParameters`. Debe proporcionar las credenciales de la cuenta de almacenamiento al montar volúmenes con el SDK de Azure Batch AI. Consulte los esquemas para el montaje de volúmenes en [Python](https://docs.microsoft.com/python/api/azure-mgmt-batchai/azure.mgmt.batchai.models.MountVolumes?view=azure-python), [C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.batchai.models.mountvolumes?view=azure-dotnet) y [Java](https://docs.microsoft.com/java/api/com.microsoft.azure.management.batchai._mount_volumes?view=azure-java-stable).

### <a name="mount-volumes-to-a-cluster"></a>Montaje de volúmenes a un clúster

Batch AI también admite el montaje de volúmenes de Azure Storage a un clúster de Batch AI. Cuando un volumen se monta en un clúster, todos los trabajos que se ejecuten en ese clúster podrán usar los volúmenes montados en él. Aunque el montaje en el nivel de trabajo proporciona la máxima flexibilidad (permitiendo tener distintos volúmenes montados en ), el montaje en el nivel de clúster debe bastar para escenarios sencillos.

Para montar un volumen de Azure Storage a un clúster creado con la CLI de Azure, use la propiedad `mountVolumes` en el archivo `cluster.json` mientras ejecuta `az batchai cluster create`. El esquema para `mountVolumes` durante el montaje a un clúster es el mismo que el necesario para el montaje a un trabajo. 

De igual modo, puede usar la propiedad `mount_volumes` (Python) o `MountVolumes` (C#, Java) en `ClusterCreateParameters` durante el montaje con el SDK de Azure Batch AI. 

## <a name="access-mounted-filesystem-in-a-job"></a>Acceso al sistema de archivos montado en un trabajo

### <a name="azbatchaijobmountroot-environment-variable"></a>Variable de entorno $AZ_BATCHAI_JOB_MOUNT_ROOT

Dentro del entorno de ejecución del trabajo, se puede acceder al directorio que contiene los sistemas de almacenamiento montado con la variable de entorno `$AZ_BATCHAI_JOB_MOUNT_ROOT` (si está usando el montaje en el nivel de trabajo). Si está usando el montaje en el nivel de clúster, la variable de entorno es `$AZ_BATCHAI_MOUNT_ROOT`. En los ejemplos siguientes, se supone que usa el montaje en el nivel de trabajo.

Para proporcionar la ruta de acceso de los datos en un volumen montado, use la variable de entorno `$AZ_BATCHAI_JOB_MOUNT_ROOT` junto con la ruta de acceso montado. Por ejemplo, si el script de entrenamiento `train.py` se cargó en un recurso compartido de archivos de Azure en la ruta de montaje relativa `scripts`, el archivo estará disponible en `$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts/train.py`.

Si el script de entrenamiento requiere conocimiento de una ruta de acceso, se debe pasar como un argumento de línea de comandos. Por ejemplo, si almacena los datos en una carpeta denominada `train_data` en un contenedor de blobs de Azure montado en la ruta de acceso `data`, podría pasar `--data-dir=$AZ_BATCHAI_JOB_MOUNT_ROOT/data/train_data` al script como un argumento de línea de comandos. En consecuencia, debe modificar el script para que acepte argumentos de línea de comandos.

### <a name="abbreviate-input-paths"></a>Abreviación de las rutas de acceso de entrada

Para abreviar las rutas de acceso de entrada como una variable de entorno, utilice la propiedad `inputDirectories` de su archivo `job.json` (o `models.JobCreateParameters.input_directories` si usa el SDK de Batch AI). El esquema para `inputDirectories` es:

```json
{
    "inputDirectories": [{
        "id": "<ID>",
        "path": "<PATH>" 
    }]
}
```

Cada ruta de acceso especificada se colocará en una variable de entorno denominada `$AZ_BATCHAI_INPUT_<ID>`. Con este método, puede simplificar las rutas de acceso a archivos o directorios de entrada. Por ejemplo, para abreviar la ruta de acceso a un script de entrenamiento: si `"id"` es `"SCRIPT"` y `"path"` es `"$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts/train.py"`, esa ruta de acceso está disponible en `$AZ_BATCHAI_INPUT_SCRIPT` en el entorno de ejecución del trabajo.

Para más información, consulte [esta página](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#input-directories).

### <a name="abbreviate-output-paths"></a>Abreviación de las rutas de acceso de salida

Para abreviar las rutas de acceso de salida como una variable de entorno, utilice la propiedad `outputDirectories` de su archivo `job.json` (o `models.JobCreateParameters.output_directories` si usa el SDK de Batch AI). Con este método, puede simplificar las rutas de acceso para los archivos de salida. El esquema para `outputDirectories` es:

```json
{
    "outputDirectories": [{
        "id": "<ID>",
        "pathPrefix": "<PATH_PREFIX>",
        "pathSuffix": "<PATH_SUFFIX>"
    }]
}
```

Cada ruta de acceso especificada se colocará en una variable de entorno denominada `$AZ_BATCHAI_OUTPUT_<ID>`. El `pathPrefix` determina el volumen montado en el que se almacenará la salida (por ejemplo, `"$AZ_BATCHAI_JOB_MOUNT_ROOT/output"`). El `pathSuffix` determina el nombre de la carpeta de la salida (por ejemplo, `"logs"`, `"checkpoints"`). La ruta de acceso real de la salida es una concatenación de `pathPrefix`, `jobOutputDirectoryPathSegment` (generado automáticamente para cada trabajo) y `pathSuffix`.

Para más información, consulte [esta página](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#output-directories).

## <a name="retrieve-job-output-from-azure-storage"></a>Recuperación de la salida del trabajo desde Azure Storage

### <a name="use-azure-portal"></a>Usar Azure Portal

Azure Portal es una manera cómoda de ver la salida de los trabajos en un explorador de archivos con interfaz gráfica de usuario. Sin embargo, si desea ver el resultado de stdout y stderr, o de una ruta de acceso en `outputDirectories`, los archivos se colocan en una ruta de acceso generada automáticamente en el volumen de Azure Storage. Para más información, siga leyendo.

### <a name="access-stdout-and-stderr-output"></a>Acceso a la salida stdout y stderr

Use la propiedad `stdOutErrPathPrefix` de `job.json` para indicar al trabajo dónde colocar los registros de ejecución y la salida stdout y stderr. Por ejemplo, si se monta un recurso compartido de archivos en la ruta de acceso de montaje relativa `outputs` y se especifica el `stdOutErrPathPrefix` en `"$AZ_BATCHAI_JOB_MOUNT_ROOT/outputs"`, entonces la salida de trabajo stdout y stderr estará disponible en `{subscription id}/{resource group}/workspaces/{workspace name}/experiments/{experiment name}/jobs/{job name}/{job uuid}/stdouterr` en el volumen montado. Se utiliza esta ruta de acceso generada automáticamente para evitar colisiones de salida entre los trabajos con el mismo nombre.

Para más información, consulte [esta página](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#standard-and-error-output).

### <a name="list-the-output-files"></a>Enumere los archivos de salida

Puede usar la CLI de Azure para enumerar los archivos de salida de un trabajo que estén disponibles con el comando `az batchai job file list`. Por ejemplo, para enumerar los archivos en el directorio de salida estándar de un trabajo, use `az batchai job file list -j <JOB_NAME> -g <RESOURCE_GROUP_NAME> -w <WORKSPACE_NAME> -e <EXPERIMENT_NAME>`.

Para obtener más información y ejemplos, consulte [esta página](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#stream-files-from-output-directories).

### <a name="stream-output-files"></a>Transmisión de archivos de salida

Puede usar la CLI de Azure para transmitir los archivos con el comando `az batchai job file stream`. Por ejemplo, para visualizar:
- Stdout: `az batchai job file stream -j <JOB_NAME> -g <RESOURCE_GROUP_NAME> -w <WORKSPACE_NAME> -e <EXPERIMENT_NAME> -f stdout.txt`
- Stderr: `az batchai job file stream -j <JOB_NAME> -g <RESOURCE_GROUP_NAME> -w <WORKSPACE_NAME> -e <EXPERIMENT_NAME> -f stderr.txt`

Para obtener más información y ejemplos, consulte [esta página](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#stream-files-from-output-directories).

## <a name="next-steps"></a>Pasos siguientes
- Para obtener más información acerca de los comandos de la CLI para interactuar con Azure Storage, consulte la [documentación de la CLI de Azure Batch AI](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md).
- Para obtener más ejemplos del uso de Batch AI, como el montaje de almacenamiento y la lectura de archivos de salida, consulte las [recetas de Jupyter Notebook para Batch AI](https://github.com/Azure/BatchAI).
- Explore otras opciones para el montaje de almacenamiento, incluidos el [montaje de un servidor NFS](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#mounting-nfs) y el [montaje de su propio clúster NFS, cifs o GlusterFS](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#mounting-nfs)