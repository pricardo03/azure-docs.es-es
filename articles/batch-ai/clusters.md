---
title: Trabajo con clústeres de Azure Batch AI | Microsoft Docs
description: Cómo elegir una configuración de clúster de Batch AI y crear y administrar un clúster de AI
services: batch-ai
documentationcenter: ''
author: johnwu10
manager: jeconnoc
editor: ''
ms.service: batch-ai
ms.topic: article
ms.date: 08/14/2018
ms.author: danlep
ms.custom: mvc
ROBOTS: NOINDEX
ms.openlocfilehash: 9af8ce84805e48dd3c91dd7fb4fcf0b136fbfc60
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53410098"
---
# <a name="work-with-batch-ai-clusters"></a>Trabajo con clústeres de Batch AI 

[!INCLUDE [batch-ai-retiring](../../includes/batch-ai-retiring.md)]

En este artículo se explica cómo trabajar con clústeres en Azure Batch AI. Presenta el concepto de clústeres, los tipos de configuraciones que son posibles y ejemplos. La mayoría de los ejemplos para crear y administrar un clúster en este artículo usan la CLI de Azure. Sin embargo, puede usar otras herramientas como Azure Portal y los SDK de Azure Batch AI para trabajar con clústeres.

Un clúster de Batch AI es uno de los diversos recursos del servicio. Consulte la [introducción a los recursos de Batch AI](resource-concepts.md) para comprender el ámbito de los clústeres del servicio.

## <a name="introduction-to-clusters"></a>Introducción a los clústeres

Un clúster de Batch AI contiene los recursos de proceso para ejecutar trabajos de aprendizaje automático y de aprendizaje de AI. Todos los nodos de un clúster tienen el mismo tamaño de máquina virtual e imagen del sistema operativo. Batch AI ofrece muchas opciones para crear clústeres que se personalizan para distintas necesidades. Por lo general, se configura un clúster diferente para cada categoría con la capacidad de procesamiento necesaria para completar un proyecto. Puede escalar y reducir el número de nodos en un clúster verticalmente según la demanda y el presupuesto. Los clústeres se pueden aprovisionar y compartir entre un equipo o los individuos pueden aprovisionar su propio clúster. 

Cada clúster se encuentra bajo un recurso de Batch AI que se denomina *área de trabajo*. Antes de aprovisionar un clúster, debe tener un área de trabajo de Batch AI configurado. Por ejemplo, si usa la CLI de Azure, use el comando [az batchai workspace create](/cli/azure/batchai/workspace?view=azure-cli-latest#az-batchai-workspace-create) para configurar un área de trabajo. 

Después de crear un clúster, puede enviar trabajos de uno en uno a una cola. Después, Batch AI controla el proceso de asignación de recursos para ejecutar los trabajos en el clúster. 

## <a name="cluster-configuration-options"></a>Opciones de configuración del clúster

Cuando planee un clúster, primero determine los requisitos de proceso. Batch AI ofrece opciones flexibles de configuración para que pueda personalizar un clúster conforme a sus necesidades. Las siguientes son las opciones principales que se deben tener en cuenta a la hora de aprovisionar un clúster:

* **Tamaño de máquina virtual**: elija cualquier [tamaño de máquina virtual](../virtual-machines/linux/sizes.md) que esté disponible en una [región admitida](https://azure.microsoft.com/global-infrastructure/services/) para los nodos de clúster. Cada clúster solamente puede contener un tamaño de máquina virtual, por lo que si las tareas requieren varios tipos de máquinas virtuales, debe aprovisionar un clúster independiente para cada tipo de requisito de proceso. Para entrenar el aprendizaje automático o los modelos de inteligencia artificial desarrollados con marcos de trabajo que aprovechan las GPU, vea [Tamaños de máquinas virtuales optimizadas para GPU](../virtual-machines/linux/sizes-gpu.md) en Azure.
  
* **Prioridad de máquina virtual**: Batch AI ofrece máquinas virtuales dedicadas o de baja prioridad para un clúster. Las máquinas virtuales dedicadas están reservadas para que las use en el clúster. La opción de baja prioridad asigna la capacidad de máquina virtual de Azure no utilizada con un ahorro significativo de costes a cambio de la posibilidad de que los trabajos se reemplacen si Azure reclama las máquinas virtuales. Los trabajos que se ejecutan durante más de 24 horas en una máquina virtual de baja prioridad también se reemplazan automáticamente. Si el presupuesto es un problema, quizás le interese usar máquinas virtuales de baja prioridad para trabajos de experimentación cortos. A continuación, cambie a las máquinas virtuales dedicadas cuando llegue el momento de ejecutar trabajos más largos.

* **Número de nodos**: Batch AI proporciona opciones manuales y de escalado automático para el número de nodos del clúster. Con la opción manual, puede controlar cuándo escalar o reducir verticalmente, de manera que pueda administrar sus propios costos de proceso. La opción de escalado automático garantiza que el clúster siempre se reduce cuando no se está usando, lo que minimiza los costos de proceso. 

  Si decide escalar el clúster manualmente, defina el destino inicial durante la creación de dicho clúster. El destino es el número de nodos que Batch AI asigna inicialmente. Más adelante, puede cambiar el número de nodos manualmente.  

  Si elige la opción de escalado automático, defina el número máximo y mínimo de nodos de destino durante la creación del clúster. El destino puede oscilar entre 0 y el número máximo de nodos que la [cuota de núcleos de Batch AI](quota-limits.md) admite. Un destino de 0 permite mantener la configuración del clúster sin que se le cobre por los costos de proceso. Si solicita un destino mayor que el que su límite de cuota admite, se producirá un error de aprovisionamiento. 

* **Imagen de máquina virtual**: de forma predeterminada, Batch AI aprovisiona las máquinas virtuales del clúster con una imagen de servidor de Ubuntu que admite cargas de trabajo de contenedor. Puede elegir otra imagen preconfigurada de Linux de Azure Marketplace, como [Data Science Virtual Machine](../machine-learning/data-science-virtual-machine/overview.md). 

* **Almacenamiento**: Batch AI proporciona una opción de almacenamiento automático, que puede elegir cuando crea un clúster mediante la CLI de Azure. Esta opción crea automáticamente un contenedor de blobs y un recurso compartido de archivos de Azure en una nueva cuenta de almacenamiento. Estos recursos de almacenamiento se montan en cada uno de los nodos del clúster durante el tiempo de ejecución, lo que permite acceder a los archivos desde una ruta de acceso local. Los nombres de cuenta de almacenamiento, el nombre del recurso compartido de archivos, el nombre del contenedor de blobs y las rutas de acceso de montaje tienen valores predeterminados, lo que también se puede personalizar mediante parámetros adicionales durante la creación del clúster. 

  Si no se definen opciones de almacenamiento, debe crear los recursos de almacenamiento por separado y definirlos al enviar los trabajos. Esta opción es útil si el clúster se administra en el nivel de organización y el almacenamiento se administra en el nivel de usuario. Para más información sobre cómo cargar archivos en Azure Storage y acceder a ellos en tiempo de ejecución, consulte [Store Batch AI job input and output with Azure Storage](use-azure-storage.md) (Almacenamiento de la entrada y salida de trabajos de Batch AI con Azure Storage).

* **Acceso de usuario**: Batch AI le permite generar archivos de clave SSH pública y privada al crear un clúster o proporcionar sus propias claves SSH, por lo que puede usar SSH en nodos individuales. También puede definir un nombre de usuario (establecido como el usuario actual de forma predeterminada) y una contraseña. Estas credenciales se pueden utilizar para acceder a los nodos durante la ejecución con el fin de ver diversas métricas o adquirir más información de los trabajos.

* **Tarea de configuración**: Batch AI le permite definir argumentos de línea de comandos que se ejecutan en cada nodo tras la asignación. También puede definir la ruta de acceso donde se debe registrar el archivo de salida. Use esta opción cuando tenga pasos de aprovisionamiento adicionales más allá de la imagen base.

* **Configuración adicional**: hay varios escenarios menos habituales que podrían requerir configuraciones más especializadas. En este caso, un [archivo de configuración de clúster](https://github.com/Azure/BatchAI/blob/master/documentation/using-azure-cli-20.md#using-cluster-configuration-file) puede asociarse al comando de la CLI de Azure para crear un clúster. 

## <a name="create-the-cluster"></a>Creación de clústeres

Después de haber decidido las opciones de configuración del clúster, use la CLI de Azure, Azure Portal o las API de Batch AI para crear el clúster. Por ejemplo, para crear un clúster mediante la CLI de Azure, puede seguir la documentación de [az batchai cluster create](/cli/azure/batchai/cluster?view=azure-cli-latest#az-batchai-cluster-create) para crear el comando exacto que le proporciona las configuraciones que necesita. 

En la configuración más elemental, el siguiente comando aprovisiona un clúster Standard_NC6 con un nodo y acceso SSH. De forma predeterminada, este clúster contiene máquinas virtuales dedicadas que ejecutan la imagen de servidor de Ubuntu más reciente.

```azurecli-interactive
az batchai cluster create \
    --name mycluster \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --vm-size Standard_NC6 \
    --target 1 \
    --generate-ssh-keys
```

En el siguiente ejemplo se aprovisiona un clúster Standard_NC6 que escala automáticamente de 0 a 4 nodos y usa nodos de baja prioridad y una cuenta de almacenamiento automático. Esta disposición es una buena configuración si necesita un clúster de bajo costo y fácil de administrar. 

```azurecli-interactive
az batchai cluster create \
    --name mycluster \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --vm-size Standard_NC6 \
    --vm-priority lowpriority \
    --max 4 \
    --min 0 \
    --use-auto-storage 
```

En el ejemplo siguiente se aprovisiona un clúster Standard_NC6 que incluye una imagen de Data Science Virtual Machine, almacenamiento personalizado y opciones de montaje, credenciales SSH personalizadas, una tarea de configuración que instala el paquete de *descompresión* y un archivo de configuración de clúster para la configuración adicional. Esta configuración es un ejemplo de un clúster más personalizado conforme a sus propias necesidades.

```azurecli-interactive
az batchai cluster create \
    --name mycluster \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --vm-size Standard_NC6 \
    --image UbuntuDSVM \ 
    --config-file cluster.json \
    --setup-task 'apt install unzip -y'
    --storage-account-name <STORAGE ACCOUNT NAME> \
    --nfs-name nfsmount \
    --afs-name afsmount \
    --bfs-name bfsmount \
    --user-name adminuser \
    --ssh-key id_rsa.pub \
    --password secretpassword 
```

## <a name="monitor-the-cluster"></a>Supervisión del clúster

Los comandos [az batchai cluster list](/cli/azure/batchai/cluster?view=azure-cli-latest#az-batchai-cluster-list), [az batchai cluster show](/cli/azure/batchai/cluster?view=azure-cli-latest#az-batchai-cluster-show) y [az batchai cluster node list](/cli/azure/batchai/cluster/node?view=azure-cli-latest#az-batchai-cluster-node-list) pueden utilizarse para supervisar diversa información para cada uno de los clústeres.

### <a name="list-all-clusters"></a>Lista de todos los clústeres

El siguiente comando enumera todos los clústeres de un área de trabajo.

```azurecli-interactive
az batchai cluster list \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> 
```

### <a name="show-information-about-a-cluster"></a>Visualización de información sobre un clúster

El siguiente comando muestra toda la información sobre un clúster concreto en un formato de tabla.

```azurecli-interactive
az batchai cluster show \
    --name <CLUSTER NAME> \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --output table
```

Si el clúster se aprovisionó mediante la opción de almacenamiento automático, querrá recuperar el nombre de la cuenta de almacenamiento para usarla al cargar los scripts y trabajos de aprendizaje. Use el comando siguiente:

```azurecli-interactive
az batchai cluster show \
    --name <CLUSTER NAME> \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> \
    --query "nodeSetup.mountVolumes.azureFileShares[0].{storageAccountName:accountName}"
```

La salida debe ser similar a la siguiente.

```JSON
{
  "storageAccountName": "baixxxxxxxxx"
}
```

### <a name="list-cluster-nodes"></a>Lista de nodos del clúster

Si necesita conectarse a los nodos del clúster, el comando siguiente recupera la lista de nodos y la información de conexión.  

```azurecli-interactive
az batchai cluster node list \
    --name <CLUSTER NAME> \
    --workspace <WORKSPACE> \
    --resource-group <RESOURCE GROUP> 
 ```

La salida de cada nodo será similar a lo siguiente:

```JSON
[
  {
    "ipAddress": "40.68.xxx.xxx",
    "nodeId": "tvm-xxxxxxxxxx-xxxxxxxx",
    "port": 50000.0
  }
]
```
Puede usar esta información para realizar una conexión SSH con un nodo mediante un comando similar al siguiente.

```bash
ssh myusername@40.68.xxx.xxx -p 50000
``` 

## <a name="submit-jobs-to-the-cluster"></a>Envío de trabajos al clúster

Después de aprovisionar el clúster, puede enviar los trabajos para que se ejecuten en los nodos. Consulte el comando [az batchai job](/cli/azure/batchai/job?view=azure-cli-latest) para las distintas formas preparar, enviar y supervisar trabajos mediante la CLI de Azure. 

## <a name="downscale-cluster-for-later-use"></a>Reducción del clúster para su uso posterior

Una vez haya terminado de ejecutar los trabajos, querrá reducir el clúster. Se recomienda reducir siempre los clústeres cuando no se usen con el fin de ahorrar costos de proceso. La reducción de un clúster a 0 nodos permite detener los cargos de facturación sin tener que reaprovisionar los clústeres en el futuro cuando necesite escalar de nuevo. Si seleccionó el escalado automático en la creación del clúster, este se debe reducir automáticamente al destino mínimo. Si se seleccionó el escalado manual, reduzca el clúster mediante el comando siguiente.

```azurecli-interactive
az batchai cluster resize \
    --name <CLUSTER NAME> \
    --resource-group <RESOURCE GROUP> \
    --workspace <WORKSPACE> \
    --target 0
```

## <a name="delete-a-cluster"></a>Eliminación de un clúster

Cuando termine de usar un clúster, use el comando [az batchai cluster delete](/cli/azure/batchai/cluster?view=azure-cli-latest#az-batchai-cluster-delete) para eliminarlo.

```azurecli-interactive
az batchai cluster delete \
    --name <CLUSTER NAME> \
    --resource-group <RESOURCE GROUP> \
    --workspace <WORKSPACE>
```

Al eliminar el grupo de recursos también se eliminan automáticamente todos los clústeres que se aprovisionaron en ese grupo de recursos.

```azurecli-interactive
az group delete --name <RESOURCE GROUP>
```

## <a name="next-steps"></a>Pasos siguientes

Para más ejemplos de creación de un clúster de Batch AI, consulte el [portal](quickstart-create-cluster-portal.md) o el inicio rápido de la [CLI de Azure](quickstart-create-cluster-cli.md), o las [recetas de Batch AI](https://github.com/Azure/BatchAI/tree/master/recipes) en GitHub.
