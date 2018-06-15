---
title: 'Guía de inicio rápido de Azure: Aprendizaje de CNTK con Batch AI mediante la CLI de Azure | Microsoft Docs'
description: Aprenda rápidamente a ejecutar un trabajo de aprendizaje de CNTK con Batch AI mediante la CLI de Azure.
services: batch-ai
documentationcenter: na
author: AlexanderYukhanov
manager: Vaman.Bedekar
editor: tysonn
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: CLI
ms.topic: quickstart
ms.date: 10/06/2017
ms.author: Alexander.Yukhanov
ms.openlocfilehash: 8d2381f710e87751bd6547c7f435080f185020d5
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34608552"
---
# <a name="run-a-cntk-training-job-using-the-azure-cli"></a>Ejecución de un trabajo de aprendizaje de CNTK mediante la CLI de Azure

La CLI de Azure 2.0 le permite crear y administrar los recursos de Batch AI: crear o eliminar servidores de archivos y clústeres de Batch AI, y enviar, finalizar o supervisar trabajos de aprendizaje.

Esta guía de inicio rápido muestra cómo crear un clúster de GPU y ejecutar un trabajo de aprendizaje mediante Microsoft Cognitive Toolkit.

El script de aprendizaje [ConvNet_MNIST.py](https://github.com/Azure/BatchAI/blob/master/recipes/CNTK/CNTK-GPU-Python/CNTK-GPU-Python.ipynb) está disponible en la página de GitHub de Batch AI. Este script entrena una red neuronal convolucional en la base de datos MNIST de dígitos manuscritos.

El ejemplo oficial de CNTK se ha modificado para aceptar la ubicación del conjunto de datos de aprendizaje y la ubicación del directorio de salida mediante argumentos de la línea de comandos.

## <a name="quickstart-overview"></a>Introducción a la guía de inicio rápido

* Cree un clúster GPU de nodo único (con un tamaño de máquina virtual `Standard_NC6`) con el nombre `nc6`.
* Cree una nueva cuenta de almacenamiento para almacenar la entrada y salida de los trabajos.
* Cree un recurso compartido de archivos de Azure con dos carpetas `logs` y `scripts` para almacenar la salida de los trabajos y los scripts de aprendizaje.
* Cree un contenedor de blobs de Azure `data` para almacenar los datos de aprendizaje.
* Implemente el script y los datos de aprendizaje en el recurso compartido de archivos y el contenedor que ha creado.
* Configure el trabajo para montar el recurso compartido de archivos de Azure y el contenedor de blobs de Azure en el nodo del clúster y hacer que estén disponibles como un sistema de archivos normal en `$AZ_BATCHAI_JOB_MOUNT_ROOT/logs`, `$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts` y `$AZ_BATCHAI_JOB_MOUNT_ROOT/data`.
`AZ_BATCHAI_JOB_MOUNT_ROOT` es una variable de entorno que establece Batch AI para el trabajo.
* Supervise la ejecución del trabajo haciendo streaming de su salida estándar.
* Después de la realización del trabajo, inspeccione la salida y los modelos generados.
* Por último, elimine todos los recursos asignados.

# <a name="prerequisites"></a>requisitos previos

* Suscripción a Azure: si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.
* Acceda a la CLI de Azure 2.0 con la versión 2.0.31 o posterior. Puede usar la CLI de Azure 2.0 disponible en [Cloud Shell](https://docs.microsoft.com/en-us/azure/cloud-shell/overview) o instalarla localmente mediante [estas instrucciones](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest).

# <a name="cloud-shell-only"></a>Solo Cloud Shell

Si va a usar Cloud Shell, cambie el directorio de trabajo a `/usr/$USER/clouddrive` ya que su directorio particular no tiene ningún espacio vacío:

```azurecli
cd /usr/$USER/clouddrive
```

# <a name="create-a-resource-group"></a>Creación de un grupo de recursos

Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure. El comando siguiente crea un nuevo grupo de recursos ```batchai.quickstart``` en la ubicación Este de EE. UU.:

```azurecli
az group create -n batchai.quickstart -l eastus
```

# <a name="create-gpu-cluster"></a>Creación del clúster de GPU

El comando siguiente crea un clúster GPU de nodo único (el tamaño de la máquina virtual es Standard_NC6) mediante Ubuntu DSVM como imagen del sistema operativo:

```azurecli
az batchai cluster create -n nc6 -g batchai.quickstart -s Standard_NC6 -i UbuntuDSVM -t 1 --generate-ssh-keys
```

Ubuntu DSVM le permite ejecutar cualquier trabajo de aprendizaje en contenedores de Docker y ejecutar las plataformas de aprendizaje profundo más conocidas directamente en la máquina virtual.

La opción `--generate-ssh-keys` le indica a la CLI de Azure que debe generar las claves SSH privadas y públicas si no dispone de ellas aún. Puede acceder a los nodos del clúster con el nombre de usuario actual y generar la clave SSH.

Si va a usar Cloud Shell, no olvide realizar una copia de seguridad de la carpeta ~/.ssh en algún almacenamiento permanente.

Salida de ejemplo:
```json
{
  "allocationState": "steady",
  "allocationStateTransitionTime": "2018-04-11T21:17:26.345000+00:00",
  "creationTime": "2018-04-11T20:12:10.758000+00:00",
  "currentNodeCount": 0,
  "errors": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/batchai.quickstart/providers/Microsoft.BatchAI/clusters/nc6",
  "location": "eastus",
  "name": "nc6",
  "nodeSetup": null,
  "nodeStateCounts": {
    "additionalProperties": {},
    "idleNodeCount": 0,
    "leavingNodeCount": 0,
    "preparingNodeCount": 0,
    "runningNodeCount": 0,
    "unusableNodeCount": 0
  },
  "provisioningState": "succeeded",
  "provisioningStateTransitionTime": "2018-04-11T20:12:11.445000+00:00",
  "resourceGroup": "batchai.quickstart",
  "scaleSettings": {
    "additionalProperties": {},
    "autoScale": null,
    "manual": {
      "nodeDeallocationOption": "requeue",
      "targetNodeCount": 1
    }
  },
  "subnet": null,
  "tags": null,
  "type": "Microsoft.BatchAI/Clusters",
  "userAccountSettings": {
    "additionalProperties": {},
    "adminUserName": "alex",
    "adminUserPassword": null,
    "adminUserSshPublicKey": "<YOUR SSH PUBLIC KEY HERE>"
  },
  "virtualMachineConfiguration": {
    "additionalProperties": {},
    "imageReference": {
      "additionalProperties": {},
      "offer": "linux-data-science-vm-ubuntu",
      "publisher": "microsoft-ads",
      "sku": "linuxdsvmubuntu",
      "version": "latest",
      "virtualMachineImageId": null
    }
  },
  "vmPriority": "dedicated",
  "vmSize": "STANDARD_NC6"
}
```

# <a name="create-a-storage-account"></a>Creación de una cuenta de almacenamiento

El siguiente comando crea una nueva cuenta de almacenamiento en la misma región que el grupo de recursos batchai.repices. Actualice el comando con un nombre de cuenta de almacenamiento único.

```azurecli
az storage account create -n <storage account name> --sku Standard_LRS -g batchai.quickstart
```

Si el nombre de la cuenta de almacenamiento seleccionado no está disponible, el comando anterior notificará el error correspondiente. En este caso, elija otro nombre e inténtelo de nuevo.

# <a name="data-deployment"></a>Implementación de los datos

## <a name="download-the-training-script-and-training-data"></a>Descargue el script y los datos de aprendizaje

* Descargue y extraiga la base de datos MNIST procesada anteriormente desde [esta ubicación](https://batchaisamples.blob.core.windows.net/samples/mnist_dataset.zip?st=2017-09-29T18%3A29%3A00Z&se=2099-12-31T08%3A00%3A00Z&sp=rl&sv=2016-05-31&sr=c&sig=PmhL%2BYnYAyNTZr1DM2JySvrI12e%2F4wZNIwCtf7TRI%2BM%3D) en la carpeta actual.

Para GNU/Linux o Cloud Shell:

```azurecli
wget "https://batchaisamples.blob.core.windows.net/samples/mnist_dataset.zip?st=2017-09-29T18%3A29%3A00Z&se=2099-12-31T08%3A00%3A00Z&sp=rl&sv=2016-05-31&sr=c&sig=PmhL%2BYnYAyNTZr1DM2JySvrI12e%2F4wZNIwCtf7TRI%2BM%3D" -O mnist_dataset.zip
unzip mnist_dataset.zip
```

Tenga en cuenta que puede que necesite instalar `unzip` si la distribución de GNU/Linux no lo tiene.

* Descargue el script de ejemplo [ConvNet_MNIST.py](https://raw.githubusercontent.com/Azure/BatchAI/master/recipes/CNTK/CNTK-GPU-Python/ConvNet_MNIST.py) en la carpeta actual:

Para GNU/Linux o Cloud Shell:

```azurecli
wget https://raw.githubusercontent.com/Azure/BatchAI/master/recipes/CNTK/CNTK-GPU-Python/ConvNet_MNIST.py
```

## <a name="create-azure-file-share-and-deploy-the-training-script"></a>Creación de un recurso compartido de archivos de Azure e implementación del script de aprendizaje

Los siguientes comandos crean los recursos compartidos de archivos de Azure `scripts` y `logs`, y copia el script de aprendizaje en la carpeta `cntk` del recurso compartido `scripts`:

```azurecli
az storage share create -n scripts --account-name <storage account name>
az storage share create -n logs --account-name <storage account name>
az storage directory create -n cntk -s scripts --account-name <storage account name>
az storage file upload -s scripts --source ConvNet_MNIST.py --path cntk --account-name <storage account name> 
```

## <a name="create-a-blob-container-and-deploy-training-data"></a>Creación de un contenedor de blobs e implementación de los datos de aprendizaje

Los siguientes comandos crean `data` de un contenedor de blobs de Azure y copian los datos de aprendizaje en la carpeta `mnist_cntk`:
```azurecli
az storage container create -n data --account-name <storage account name>
az storage blob upload-batch -s . --pattern '*28x28_cntk*' --destination data --destination-path mnist_cntk --account-name <storage account name>
```

# <a name="submit-training-job"></a>Envío de un trabajo de aprendizaje

## <a name="prepare-job-configuration-file"></a>Preparación de un archivo de configuración de trabajos

Cree un archivo de configuración del trabajo de aprendizaje `job.json` con el siguiente contenido:
```json
{
    "$schema": "https://raw.githubusercontent.com/Azure/BatchAI/master/schemas/2018-03-01/cntk.json",
    "properties": {
        "nodeCount": 1,
        "cntkSettings": {
            "pythonScriptFilePath": "$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts/cntk/ConvNet_MNIST.py",
            "commandLineArgs": "$AZ_BATCHAI_JOB_MOUNT_ROOT/data/mnist_cntk $AZ_BATCHAI_OUTPUT_MODEL"
        },
        "stdOutErrPathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/logs",
        "outputDirectories": [{
            "id": "MODEL",
            "pathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/logs"
        }],
        "mountVolumes": {
            "azureFileShares": [
                {
                    "azureFileUrl": "https://<AZURE_BATCHAI_STORAGE_ACCOUNT>.file.core.windows.net/logs",
                    "relativeMountPath": "logs"
                },
                {
                    "azureFileUrl": "https://<AZURE_BATCHAI_STORAGE_ACCOUNT>.file.core.windows.net/scripts",
                    "relativeMountPath": "scripts"
                }
            ],
            "azureBlobFileSystems": [
                {
                    "accountName": "<AZURE_BATCHAI_STORAGE_ACCOUNT>",
                    "containerName": "data",
                    "relativeMountPath": "data"
                }
            ]
        }
    }
}
```

Este archivo de configuración especifica:

* `nodeCount`: el número de nodos que requiere el trabajo (1 para esta guía de inicio rápido).
* `cntkSettings`: especifica la ruta de acceso del script de aprendizaje y los argumentos de la línea de comandos. Los argumentos de la línea de comandos incluyen la ruta a los datos de aprendizaje y la ruta de destino para almacenar los modelos generados. `AZ_BATCHAI_OUTPUT_MODEL` es una variable de entorno que establece Batch AI basándose en la configuración del directorio de salida (consulte a continuación).
* `stdOutErrPathPrefix`: la ruta de acceso en la que Batch AI creará los directorios que contendrán las salidas y registros del trabajo.
* `outputDirectories`: la colección de directorios de salida que va a crear Batch AI. Para cada directorio, Batch AI crea una variable de entorno con nombre `AZ_BATCHAI_OUTPUT_<id>`, donde `<id>` es el identificador del directorio.
* `mountVolumes`: lista de sistemas de archivos que se va a montar durante la ejecución del trabajo. Los sistemas de archivos se montan en `AZ_BATCHAI_JOB_MOUNT_ROOT/<relativeMountPath>`. `AZ_BATCHAI_JOB_MOUNT_ROOT` es una variable de entorno que establece Batch AI.
* `<AZURE_BATCHAI_STORAGE_ACCOUNT>` indica que el nombre de la cuenta de almacenamiento se especificará durante el envío del trabajo mediante el parámetro --storage-account-name o la variable de entorno `AZURE_BATCHAI_STORAGE_ACCOUNT` del equipo.

## <a name="submit-the-job"></a>Envío del trabajo

Use el siguiente comando para enviar el trabajo del clúster:

```azurecli
az batchai job create -n cntk_python_1 -r nc6 -g batchai.quickstart -c job.json --storage-account-name <storage account name>
```

Salida de ejemplo:
```
{
  "additionalProperties": {},
  "caffeSettings": null,
  "chainerSettings": null,
  "cluster": {
    "additionalProperties": {},
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/batchai.quickstart/providers/Microsoft.BatchAI/clusters/nc6",
    "resourceGroup": "batchai.quickstart"
  },
  "cntkSettings": {
    "additionalProperties": {},
    "commandLineArgs": "$AZ_BATCHAI_JOB_MOUNT_ROOT/data/mnist_cntk $AZ_BATCHAI_OUTPUT_MODEL",
    "configFilePath": null,
    "languageType": "Python",
    "processCount": 1,
    "pythonInterpreterPath": null,
    "pythonScriptFilePath": "$AZ_BATCHAI_JOB_MOUNT_ROOT/scripts/cntk/ConvNet_MNIST.py"
  },
  "constraints": {
    "additionalProperties": {},
    "maxWallClockTime": "7 days, 0:00:00"
  },
  "containerSettings": null,
  "creationTime": "2018-04-11T21:48:10.303000+00:00",
  "customToolkitSettings": null,
  "environmentVariables": null,
  "executionInfo": null,
  "executionState": "queued",
  "executionStateTransitionTime": "2018-04-11T21:48:10.303000+00:00",
  "experimentName": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/batchai.quickstart/providers/Microsoft.BatchAI/jobs/cntk_python_1",
  "inputDirectories": null,
  "jobOutputDirectoryPathSegment": "00000000-0000-0000-0000-000000000000/batchai.quickstart/jobs/cntk_python_1/b9576bae-e878-4fb2-9390-2e962356b5b1",
  "jobPreparation": null,
  "location": null,
  "mountVolumes": {
    "additionalProperties": {},
    "azureBlobFileSystems": [
      {
        "accountName": "<YOU STORAGE ACCOUNT NAME>",
        "additionalProperties": {},
        "containerName": "data",
        "credentials": {
          "accountKey": null,
          "accountKeySecretReference": null,
          "additionalProperties": {}
        },
        "mountOptions": null,
        "relativeMountPath": "data"
      }
    ],
    "azureFileShares": [
      {
        "accountName": "<YOU STORAGE ACCOUNT NAME>,
        "additionalProperties": {},
        "azureFileUrl": "https://<YOU STORAGE ACCOUNT NAME>.file.core.windows.net/logs",
        "credentials": {
          "accountKey": null,
          "accountKeySecretReference": null,
          "additionalProperties": {}
        },
        "directoryMode": "0777",
        "fileMode": "0777",
        "relativeMountPath": "logs"
      },
      {
        "accountName": "<YOU STORAGE ACCOUNT NAME>",
        "additionalProperties": {},
        "azureFileUrl": "https://<YOU STORAGE ACCOUNT NAME>.file.core.windows.net/scripts",
        "credentials": {
          "accountKey": null,
          "accountKeySecretReference": null,
          "additionalProperties": {}
        },
        "directoryMode": "0777",
        "fileMode": "0777",
        "relativeMountPath": "scripts"
      }
    ],
    "fileServers": null,
    "unmanagedFileSystems": null
  },
  "name": "cntk_python_1",
  "nodeCount": 1,
  "outputDirectories": [
    {
      "additionalProperties": {},
      "createNew": true,
      "id": "MODEL",
      "pathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/logs",
      "pathSuffix": null,
      "type": "custom"
    }
  ],
  "priority": 0,
  "provisioningState": "succeeded",
  "provisioningStateTransitionTime": "2018-04-11T21:48:11.577000+00:00",
  "pyTorchSettings": null,
  "resourceGroup": "batchai.quickstart",
  "secrets": null,
  "stdOutErrPathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/logs",
  "tags": null,
  "tensorFlowSettings": null,
  "toolType": "cntk",
  "type": "Microsoft.BatchAI/Jobs"
}
```

# <a name="monitor-job-execution"></a>Supervisión de la ejecución de trabajos

El script de aprendizaje informa sobre el progreso de este en el archivo `stderr.txt` situado en el directorio de salida estándar. Puede supervisar el progreso mediante el comando siguiente:

```azurecli
az batchai job file stream -n cntk_python_1 -g batchai.quickstart -f stderr.txt
```

Salida de ejemplo:
```
File found with URL "https://<YOU STORAGE ACCOUNT>.file.core.windows.net/logs/00000000-0000-0000-0000-000000000000/batchai.quickstart/jobs/cntk_python_1/<JOB's UUID>/stdouterr/stderr.txt?sv=2016-05-31&sr=f&sig=n86JK9YowV%2BPQ%2BkBzmqr0eud%2FlpRB%2FVu%2FFlcKZx192k%3D&se=2018-04-11T23%3A05%3A54Z&sp=rl". Start streaming
Selected GPU[0] Tesla K80 as the process wide default device.
-------------------------------------------------------------------
Build info:

        Built time: Jan 31 2018 15:03:41
        Last modified date: Tue Jan 30 03:26:13 2018
        Build type: release
        Build target: GPU
        With 1bit-SGD: no
        With ASGD: yes
        Math lib: mkl
        CUDA version: 9.0.0
        CUDNN version: 7.0.4
        Build Branch: HEAD
        Build SHA1: a70455c7abe76596853f8e6a77a4d6de1e3ba76e
        MPI distribution: Open MPI
        MPI version: 1.10.7
-------------------------------------------------------------------
Training 98778 parameters in 10 parameter tensors.

Learning rate per 1 samples: 0.001
Momentum per 1 samples: 0.0
Finished Epoch[1 of 40]: [Training] loss = 0.405960 * 60000, metric = 13.01% * 60000 21.741s (2759.8 samples/s);
Finished Epoch[2 of 40]: [Training] loss = 0.106030 * 60000, metric = 3.09% * 60000 3.638s (16492.6 samples/s);
Finished Epoch[3 of 40]: [Training] loss = 0.078542 * 60000, metric = 2.32% * 60000 3.477s (17256.3 samples/s);
...
Final Results: Minibatch[1-11]: errs = 0.54% * 10000
```

El streaming se detiene cuando se termina el trabajo (ya sea correctamente o con errores).

# <a name="inspect-generated-model-files"></a>Revisión de los archivos de modelos generados

El trabajo almacena los archivos de modelos generados en el directorio de salida con un atributo `id` que equivale a `MODEL`. Puede enumerar los archivos de modelos y obtener las direcciones URL de descarga mediante el comando siguiente:

```azurecli
az batchai job file list -n cntk_python_1 -g batchai.quickstart -d MODEL
```

Salida de ejemplo:
```
[
  {
    "additionalProperties": {},
    "contentLength": 409456,
    "downloadUrl": "https://<YOUR STORAGE ACCOUNT>.file.core.windows.net/...",
    "isDirectory": false,
    "lastModified": "2018-04-11T22:05:51+00:00",
    "name": "ConvNet_MNIST_0.dnn"
  },
  {
    "additionalProperties": {},
    "contentLength": 409456,
    "downloadUrl": "https://<YOUR STORAGE ACCOUNT>.file.core.windows.net/...",
    "isDirectory": false,
    "lastModified": "2018-04-11T22:05:55+00:00",
    "name": "ConvNet_MNIST_1.dnn"
  },
...

```

También puede usar el portal o el Explorador de Azure Storage para revisar los archivos generados. Para diferenciar la salida de los diferentes trabajos, Batch AI crea una estructura de carpetas única para cada una de ellas. Puede encontrar la ruta de acceso a la carpeta que contiene la salida mediante el atributo `jobOutputDirectoryPathSegment` del trabajo enviado:

```azurecli
az batchai job show -n cntk_python_1 -g batchai.quickstart --query jobOutputDirectoryPathSegment
```

Salida de ejemplo:
```
"00000000-0000-0000-0000-000000000000/batchai.quickstart/jobs/cntk_python_1/<JOB's UUID>"
```

# <a name="delete-resources"></a>Eliminación de recursos

Elimine el grupo de recursos y todos los recursos asignados con el siguiente comando:

```azurecli
az group delete -n batchai.quickstart -y
```
