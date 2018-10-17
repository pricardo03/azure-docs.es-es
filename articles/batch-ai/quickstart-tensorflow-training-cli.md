---
title: 'Guía de inicio rápido de Azure: entrenamiento del aprendizaje profundo con la CLI de Azure | Microsoft Docs'
description: 'Guía de inicio rápido: aprenda rápidamente a entrenar una red neuronal de aprendizaje profundo de TensorFlow en una unidad GPU única con Batch AI mediante la CLI de Azure'
services: batch-ai
documentationcenter: na
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.custom: ''
ms.service: batch-ai
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: CLI
ms.topic: quickstart
ms.date: 09/03/2018
ms.author: danlep
ms.openlocfilehash: 99d864a5d519ce56a559bea4db7fe89a113e47b9
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44157929"
---
# <a name="quickstart-train-a-deep-learning-model-with-batch-ai"></a>Guía de inicio rápido: entrenamiento de un modelo de aprendizaje profundo con Batch AI

Esta guía de inicio rápido muestra cómo entrenar un modelo de aprendizaje profundo de ejemplo en una máquina virtual con GPU mediante Batch AI. Batch AI es un servicio administrado que permite a los científicos de datos y a los investigadores de la AI entrenar modelos de AI y aprendizaje automático a escala en clústeres de máquinas virtuales de Azure. 

En este ejemplo se usa la CLI de Azure para configurar Batch AI para entrenar una red neuronal de [TensorFlow](https://www.tensorflow.org/) de ejemplo en la [base de datos MNIST](http://yann.lecun.com/exdb/mnist/) de dígitos manuscritos. Después de completar esta guía de inicio rápido, entenderá los conceptos clave de usar Batch AI para entrenar un modelo de AI o de aprendizaje automático y estará listo para intentar entrenar distintos modelos a mayor escala.

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI en un entorno local, para esta guía de inicio rápido es preciso que ejecute la versión 2.0.38 de la CLI de Azure o una posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). 

En esta guía de inicio rápido se da por supuesto que ejecuta los comandos en un shell de Bash, ya sea en Cloud Shell o en la máquina local. Si ya se ha completado la guía de inicio rápido para [crear un clúster de Batch AI con la CLI de Azure](quickstart-create-cluster-cli.md), omita los dos primeros pasos de creación de un grupo de recursos y un clúster de Batch AI.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos con el comando `az group create`. Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure. 

En el ejemplo siguiente se crea un grupo de recursos denominado *myResourceGroup* en la ubicación *eastus2*. Asegúrese de elegir la ubicación Este de EE. UU. 2 u otra ubicación donde el servicio Batch AI está disponible. 

```azurecli-interactive 
az group create \
    --name myResourceGroup \
    --location eastus2
```

## <a name="create-a-batch-ai-cluster"></a>Creación de un clúster de Batch AI

En primer lugar, use el comando `az batchai workspace create` para crear un *área de trabajo* de Batch AI. Necesita un área de trabajo para organizar los clústeres de Batch AI y otros recursos.

```azurecli-interactive
az batchai workspace create \
    --workspace myworkspace \
    --resource-group myResourceGroup
```

Para crear un clúster de Batch AI, use el comando `az batchai cluster create`. En el siguiente ejemplo se crea un clúster de un solo nodo con las siguientes propiedades:

* Usa una máquina virtual de tamaño NC6 y una unidad GPU NVIDIA Tesla K80. Azure ofrece varios tamaños de máquina virtual con distintas unidades GPU de NVIDIA.
* Ejecuta una imagen de Ubuntu Server predeterminada diseñada para hospedar aplicaciones en contenedores. Puede ejecutar la mayoría de las cargas de trabajo de entrenamiento en esta distribución. 
* Agrega una cuenta de usuario denominada *myusername* y genera claves SSH si aún no existen en la ubicación predeterminada para las claves (*~/.ssh*) en el entorno local.

```azurecli-interactive
az batchai cluster create \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --vm-size Standard_NC6 \
    --target 1 \
    --user-name myusername \
    --generate-ssh-keys
```

La salida del comando muestra las propiedades del clúster. La creación y el inicio del nodo tardan unos minutos. Para ver el estado del clúster, ejecute el comando `az batchai cluster show`.

```azurecli-interactive
az batchai cluster show \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --output table
```

En un paso anterior de la creación del clúster, la salida se parece a la siguiente y muestra el clúster como `resizing`:

```bash
Name       Resource Group    Workspace    VM Size       State      Idle    Running    Preparing    Leaving    Unusable
---------  ----------------  -----------  ------------  -------  ------  ---------  -----------  ---------  ----------
mycluster  myResourceGroup   myworkspace  STANDARD_NC6  resizing      0          0            0          0           0

```

Siga los pasos a continuación para cargar el script de entrenamiento y crear el trabajo de entrenamiento mientras el estado del clúster cambia. El clúster está preparado para ejecutar el trabajo de entrenamiento cuando el estado es `steady` y el nodo único, `Idle`.

## <a name="upload-training-script"></a>Carga del script de entrenamiento

Use el comando `az storage account create` para crear una cuenta de almacenamiento, y almacenar el script y la salida del entrenamiento.

```azurecli-interactive
az storage account create \
    --resource-group myResourceGroup \
    --name mystorageaccount \
    --location eastus2 \
    --sku Standard_LRS
```

Cree un recurso compartido de archivos de Azure denominado `myshare` en la cuenta mediante el comando `az storage share create`:

```azurecli-interactive
az storage share create \
    --name myshare \
    --account-name mystorageaccount
```

Use el comando `az storage directory create` para crear directorios en el recurso compartido de archivos de Azure. Cree el directorio `scripts` para el script de entrenamiento y `logs` como salida del entrenamiento:

```azurecli-interactive
# Create /scripts directory in file share
az storage directory create \
    --name scripts \
    --share-name myshare \
    --account-name mystorageaccount

# Create /logs directory in file share 
az storage directory create \
    --name logs \
    --share-name myshare \
    --account-name mystorageaccount
```

En el shell de Bash, cree un directorio de trabajo local y descargue el ejemplo [convolutional.py](https://raw.githubusercontent.com/tensorflow/models/master/tutorials/image/mnist/convolutional.py) de TensorFlow. Este script de Python entrena una red neuronal convolucional en el conjunto de imágenes MNIST de 60 000 dígitos manuscritos del 0 al 9. Después, prueba el modelo en un conjunto de ejemplos de prueba.

```bash
wget https://raw.githubusercontent.com/tensorflow/models/master/tutorials/image/mnist/convolutional.py
```

Cargue el script en el directorio `scripts` en el recurso compartido mediante el comando `az storage file upload`.

```azurecli-interactive
az storage file upload \
    --share-name myshare \
    --path scripts \
    --source convolutional.py \
    --account-name mystorageaccount
```

## <a name="submit-training-job"></a>Envío de un trabajo de aprendizaje

En primer lugar, cree un *experimento* de Batch AI en el área de trabajo mediante el comando `az batchai experiment create`. Un experimento es un contenedor lógico para todos los trabajos relacionados de Batch AI.

```azurecli-interactive
az batchai experiment create \
    --name myexperiment \
    --workspace myworkspace \
    --resource-group myResourceGroup
```

En el directorio de trabajo, cree un archivo de configuración del trabajo de aprendizaje `job.json` con el siguiente contenido. Pase este archivo de configuración al enviar el trabajo de entrenamiento.

Este archivo `job.json` incluye la configuración para buscar el archivo de script de Python y ejecutarlo en un contenedor de TensorFlow en el nodo de la unidad GPU. También especifica dónde guardar los archivos de salida del trabajo en el almacenamiento de Azure. `<AZURE_BATCHAI_STORAGE_ACCOUNT>` indica que se especificará el nombre de la cuenta de almacenamiento durante el envío del trabajo.

```json
{
    "$schema": "https://raw.githubusercontent.com/Azure/BatchAI/master/schemas/2018-05-01/job.json",
    "properties": {
        "nodeCount": 1,
        "tensorFlowSettings": {
            "pythonScriptFilePath": "$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/scripts/convolutional.py"
        },
        "stdOutErrPathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/logs",
        "mountVolumes": {
            "azureFileShares": [
                {
                    "azureFileUrl": "https://<AZURE_BATCHAI_STORAGE_ACCOUNT>.file.core.windows.net/myshare",
                    "relativeMountPath": "myshare"
                }
            ]
        },
        "containerSettings": {
            "imageSourceRegistry": {
                "image": "tensorflow/tensorflow:1.8.0-gpu"
            }
        }
    }
}
```

Envíe el trabajo del nodo con el comando `az batchai job create`, que pasa el archivo de configuración `job.json` y el nombre de la cuenta de almacenamiento:

```azurecli-interactive
az batchai job create \
    --name myjob \
    --cluster mycluster \
    --experiment myexperiment \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --config-file job.json \
    --storage-account-name mystorageaccount
```

El comando devuelve las propiedades del trabajo y después tarda unos minutos en completarse. Para supervisar el progreso de este trabajo, use el comando `az batchai job file stream` para transmitir el archivo `stdout-wk-0.txt` desde el directorio de salida estándar del nodo. El script de entrenamiento genera este archivo después de que el trabajo empieza a ejecutarse.  

```azurecli-interactive
az batchai job file stream \
    --job myjob \
    --experiment myexperiment \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --file-name stdout-wk-0.txt
```

Salida de ejemplo:

```
File found with URL "https://mystorageaccount.file.core.windows.net/logs/00000000-0000-0000-0000-000000000000/myResourceGroup/workspaces/myworkspace/experiments/myexperiment/jobs/myjob/<JOB_ID>/stdouterr/stdout-wk-0.txt?sv=2016-05-31&sr=f&sig=Kih9baozMao8Ugos%2FVG%2BcsVsSeY1O%2FTocCNvLQhwtx4%3D&se=2018-06-20T22%3A07%3A30Z&sp=rl". Start streaming
Successfully downloaded train-images-idx3-ubyte.gz 9912422 bytes.
Successfully downloaded train-labels-idx1-ubyte.gz 28881 bytes.
Successfully downloaded t10k-images-idx3-ubyte.gz 1648877 bytes.
Successfully downloaded t10k-labels-idx1-ubyte.gz 4542 bytes.
Extracting data/train-images-idx3-ubyte.gz
Extracting data/train-labels-idx1-ubyte.gz
Extracting data/t10k-images-idx3-ubyte.gz
Extracting data/t10k-labels-idx1-ubyte.gz
Initialized!
Step 0 (epoch 0.00), 14.9 ms
Minibatch loss: 8.334, learning rate: 0.010000
Minibatch error: 85.9%
Validation error: 84.6%
Step 100 (epoch 0.12), 9.7 ms
Minibatch loss: 3.240, learning rate: 0.010000
Minibatch error: 6.2%
Validation error: 7.7%
Step 200 (epoch 0.23), 8.3 ms
Minibatch loss: 3.335, learning rate: 0.010000
Minibatch error: 7.8%
Validation error: 4.5%
Step 300 (epoch 0.35), 8.3 ms
Minibatch loss: 3.157, learning rate: 0.010000
Minibatch error: 3.1%
...
Step 8500 (epoch 9.89), 8.3 ms
Minibatch loss: 1.605, learning rate: 0.006302
Minibatch error: 0.0%
Validation error: 0.9%
Test error: 0.8%
```

La transmisión se detiene cuando el trabajo finaliza. El script de ejemplo entrena en más de 10 *épocas* o pasa por el conjunto de datos de entrenamiento. En este ejemplo, después de 10 épocas, el modelo entrenado funciona con un error de prueba de solo el 0,8 %.

## <a name="get-job-output"></a>Obtención de la salida del trabajo

Batch AI crea una estructura de carpeta única en la cuenta de almacenamiento para la salida de cada trabajo. Establezca la variable de entorno JOB_OUTPUT_PATH con esta ruta de acceso. A continuación, enumere los archivos de salida del almacenamiento mediante el comando `az storage file list`:

```azurecli-interactive
export JOB_OUTPUT_PATH=$(az batchai job show --name myjob --experiment myexperiment --workspace myworkspace --resource-group myResourceGroup --query jobOutputDirectoryPathSegment --output tsv)

az storage file list \
    --share-name myshare/logs \
    --account-name mystorageaccount \
    --path $JOB_OUTPUT_PATH/stdouterr \
    --output table
```

La salida es parecida a esta:

```
Name               Content Length  Type    Last Modified
---------------  ----------------  ------  ---------------
execution.log               14866  file
stderr-wk-0.txt              1527  file
stdout-wk-0.txt             11027  file
```

Use el comando `az storage file download` para descargar uno o varios archivos en el directorio de trabajo local. Por ejemplo: 

```azurecli-interactive
az storage file download \
    --share-name myshare/logs \
    --account-name mystorageaccount \
    --path $JOB_OUTPUT_PATH/stdouterr/stdout-wk-0.txt
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Si desea continuar con los ejemplos y tutoriales de Batch AI, utilice el área de trabajo de Batch AI, el clúster y la cuenta de almacenamiento que se crearon en esta guía de inicio rápido. 

Se cobra por el clúster de Batch AI mientras los nodos se ejecuten. Si desea conservar la configuración del clúster cuando no tenga trabajos para ejecutar, cambie el tamaño del clúster a 0 nodos. 

```azurecli-interactive
az batchai cluster resize \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --target 0
```

Más adelante, vuelva a cambiarlo de tamaño a 1 o más nodos para ejecutar los trabajos. Cuando ya no necesite un clúster, elimínelo con el comando `az batchai cluster delete`:

```azurecli-interactive
az batchai cluster delete \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup
```

Cuando ya no lo necesite, puede usar el comando `az group delete` para eliminar el grupo de recursos con los recursos de Batch AI y de almacenamiento. 

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes
En esta guía de inicio rápido aprendió a usar Batch AI para entrenar un modelo de ejemplo de aprendizaje profundo de TensorFlow en una única máquina virtual con GPU mediante la CLI de Azure. Para información acerca de cómo distribuir el entrenamiento de modelos en un clúster con GPU mayor, continúe con el tutorial de Batch AI.

> [!div class="nextstepaction"]
> [Tutorial sobre el entrenamiento distribuido](./tutorial-horovod-tensorflow.md)