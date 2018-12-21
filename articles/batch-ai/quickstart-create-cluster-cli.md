---
title: 'Guía de inicio rápido de Azure: creación de un clúster de Batch AI con la CLI de Azure | Microsoft Docs'
description: 'Guía de inicio rápido: creación de un clúster de Batch AI para el entrenamiento de modelos de aprendizaje automático e inteligencia artificial con la CLI de Azure'
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
ROBOTS: NOINDEX
ms.openlocfilehash: 1ea12c9a544704ea91b85ae944e611e6769b5592
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53407140"
---
# <a name="quickstart-create-a-cluster-for-batch-ai-training-jobs-using-the-azure-cli"></a>Guía de inicio rápido: Creación de un clúster para trabajos de entrenamiento de Batch AI con la CLI de Azure

[!INCLUDE [batch-ai-retiring](../../includes/batch-ai-retiring.md)]

En esta guía de inicio rápido se muestra cómo usar la CLI de Azure para crear un clúster de Batch AI que sirve para el entrenamiento de modelos de aprendizaje automático e inteligencia artificial. Batch AI es un servicio administrado que permite a los científicos de datos y a los investigadores de la AI entrenar modelos de AI y aprendizaje automático a escala en clústeres de máquinas virtuales de Azure.

Inicialmente, el clúster tiene un único nodo con GPU. Al completar esta guía de inicio rápido tendrá un clúster que podrá usar para entrenar modelos y que podrá escalar verticalmente. Envíe trabajos de entrenamiento al clúster con las herramientas de Batch AI o [Azure Machine Learning](../machine-learning/service/overview-what-is-azure-ml.md), o [Visual Studio Tools for AI](https://github.com/Microsoft/vs-tools-for-ai).

[!INCLUDE [quickstarts-free-trial-note.md](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI en un entorno local, para esta guía de inicio rápido es preciso que ejecute la versión 2.0.38 de la CLI de Azure o una posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, vea [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). 

En esta guía de inicio rápido se da por supuesto que ejecuta los comandos en un shell de Bash, ya sea en Cloud Shell o en la máquina local.

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Cree un grupo de recursos con el comando `az group create`. Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure. 

En el ejemplo siguiente se crea un grupo de recursos denominado *myResourceGroup* en la ubicación *eastus2*. Asegúrese de elegir una ubicación como Este de EE. UU. 2 en la que el servicio Batch AI esté disponible.

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

Para crear un clúster de Batch AI, use el comando `az batchai cluster create`. En el siguiente ejemplo se crea un clúster con las siguientes propiedades:

* Contiene un único nodo con una máquina virtual de tamaño NC6 y una unidad GPU NVIDIA Tesla K80. 
* Ejecuta una imagen de Ubuntu Server predeterminada diseñada para hospedar aplicaciones en contenedores que puede usar para la mayoría de las cargas de trabajo de entrenamiento. 
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

En un paso anterior de la creación del clúster, la salida se parece a la siguiente, con el clúster en estado `resizing`:

```bash
Name       Resource Group    Workspace    VM Size       State      Idle    Running    Preparing    Leaving    Unusable
---------  ----------------  -----------  ------------  -------  ------  ---------  -----------  ---------  ----------
mycluster  myResourceGroup   myworkspace  STANDARD_NC6  resizing      0          0            0          0           0

```
El clúster está preparado para usar cuando el estado es `steady` y el único nodo, `Idle`.

## <a name="list-cluster-nodes"></a>Lista de nodos del clúster 

Si necesita conectarse a los nodos del clúster (en este caso, un único nodo) para realizar tareas de mantenimiento o instalar aplicaciones, ejecute el comando `az batchai cluster node list`, que le proporcionará información de conexión:


```azurecli-interactive
az batchai cluster node list \
    --cluster mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup 
 ```

La salida JSON es parecida a esta:

```JSON
[
  {
    "ipAddress": "40.68.254.143",
    "nodeId": "tvm-1816144089_1-20180626t233430z",
    "port": 50000.0
  }
]
```
Utilice esta información para establecer una conexión SSH al nodo. Por ejemplo, sustituya la dirección IP correcta del nodo en el siguiente comando:

```bash
ssh myusername@40.68.254.143 -p 50000
``` 
Salga de la sesión SSH para continuar.

## <a name="resize-the-cluster"></a>Cambio de tamaño del clúster

Para usar el clúster para ejecutar un trabajo de entrenamiento puede que necesite más recursos de proceso. Por ejemplo, para aumentar el tamaño a 2 nodos para un trabajo de entrenamiento distribuido, ejecute el comando [batch ai cluster resize](/cli/azure/batchai/cluster#az-batchai-cluster-resize):

```azurecli-interactive
az batchai cluster resize \
    --name mycluster \
    --workspace myworkspace \
    --resource-group myResourceGroup \
    --target 2
```

El cambio de tamaño del clúster tarda unos minutos.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si desea continuar con los ejemplos y tutoriales de Batch AI, utilice el área de trabajo de Batch AI que se creó en esta guía de inicio rápido. 

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
    --resource-group myResourceGroup \
```

Cuando ya no lo necesite, puede usar el comando `az group delete` para eliminar el grupo de recursos con los recursos de Batch AI. 

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido ha aprendido a crear un clúster de Batch AI mediante la CLI de Azure. Para información sobre cómo usar un clúster de Batch AI para entrenar un modelo, continúe con la guía de inicio rápido sobre el entrenamiento de un modelo de aprendizaje profundo.

> [!div class="nextstepaction"]
> [Entrenamiento de un modelo de aprendizaje profundo](./quickstart-tensorflow-training-cli.md)
