---
title: Cargas de trabajo de contenedor en Azure Batch | Microsoft Docs
description: Aprenda a ejecutar aplicaciones desde imágenes de contenedor en Azure Batch.
services: batch
author: dlepow
manager: jeconnoc
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.workload: na
ms.date: 10/24/2018
ms.author: danlep
ms.openlocfilehash: 458b0f7bbf581c7f2490a8122f351dac612b4ff0
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50155633"
---
# <a name="run-container-applications-on-azure-batch"></a>Ejecución de aplicaciones de contenedor en Azure Batch

Azure Batch permite ejecutar y escalar un gran número de trabajos de computación por lotes en Azure. Las tareas por lotes se pueden ejecutar directamente en las máquinas virtuales (nodos) de un grupo de Batch, pero también se puede configurar un grupo de Batch para ejecutar tareas en contenedores compatibles con Docker en los nodos. En este artículo se muestra cómo crear un grupo de nodos de ejecución que admita la ejecución de tareas de contenedor y luego ejecutar las tareas de contenedor en el grupo. 

Debe estar familiarizado con los conceptos de contenedor y cómo crear un trabajo y un grupo de Batch. Los ejemplos de código usan los SDK de Python, .NET y Batch. También puede usar otras herramientas y SDK de Batch, incluido Azure Portal, para crear grupos de Batch habilitados para contenedor y para ejecutar tareas de contenedor.

## <a name="why-use-containers"></a>¿Por qué usar contenedores?

El uso de contenedores proporciona una manera sencilla de ejecutar tareas de Batch sin tener que administrar un entorno ni dependencias para ejecutar las aplicaciones. Los contenedores implementan aplicaciones como unidades ligeras, portátiles y autosuficientes que se pueden ejecutar en varios entornos distintos. Por ejemplo, puede crear y probar localmente un contenedor y, después cargar la imagen de contenedor en un registro en Azure o en otro lugar. El modelo de implementación de contenedor garantiza que el entorno en tiempo de ejecución de la aplicación siempre esté correctamente instalado y configurado donde sea que se hospede la aplicación. Las tareas basadas en contenedor en Batch también pueden aprovechar las características de tareas no en contenedor, incluidos los paquetes de aplicación y la administración de archivos de recursos y archivos de salida. 

## <a name="prerequisites"></a>Requisitos previos

* **Versiones del SDK**: los SDK de Batch admiten las imágenes de contenedor en las siguientes versiones:
    * API de REST de Batch versión 2017-09-01.6.0
    * SDK de .NET de Batch versión 8.0.0
    * SDK de Python de Batch versión 4.0
    * SDK de Java de Batch versión 3.0
    * SDK de Node.js de Batch versión 3.0

* **Cuentas**: en la suscripción de Azure, debe crear una cuenta de Batch y, opcionalmente, una cuenta de Azure Storage.

* **Una imagen de VM admitidas**: los contenedores solo se admiten en grupos creados con la configuración de máquinas virtuales a partir de imágenes que se detallan en la siguiente sección, "Imágenes de máquinas virtuales admitidas". Si proporciona una imagen personalizada, consulte las consideraciones en la siguiente sección y los requisitos en [Uso de una imagen personalizada administrada para crear un grupo de máquinas virtuales](batch-custom-images.md). 

### <a name="limitations"></a>Limitaciones

* El servicio Batch proporciona compatibilidad con RDMA solo para contenedores que se ejecutan en grupos Linux

* Para las cargas de trabajo de contenedor de Windows, se recomienda elegir un tamaño de máquina virtual multinúcleo para el grupo

## <a name="supported-virtual-machine-images"></a>Imágenes de máquinas virtuales admitidas

Use una de las siguientes imágenes Windows o Linux compatibles para crear un grupo de nodos de ejecución de máquinas virtuales para cargas de trabajo de contenedor. Para más información sobre las imágenes de Marketplace compatibles con Batch, consulte la [lista de imágenes de máquinas virtuales](batch-linux-nodes.md#list-of-virtual-machine-images). 

### <a name="windows-images"></a>Imágenes de Windows

Para las cargas de trabajo de contenedor de Windows, Batch actualmente admite la imagen **Windows Server 2016 Datacenter con Containers** en Azure Marketplace. Solo las imágenes de contenedor de Docker son compatibles con Windows.

También puede crear imágenes personalizadas desde máquinas virtuales que ejecutan Docker en Windows.

### <a name="linux-images"></a>Imágenes de Linux

En el caso de las cargas de trabajo de contenedor Linux, Batch actualmente admite estas imágenes de Linux que Microsoft Azure Batch publica en Azure Marketplace:

* **CentOS para grupos de contenedores de Azure Batch**

* **CentOS (con controladores RDMA) para grupos de contenedores de Azure Batch**

* **Ubuntu Server para grupos de contenedores de Azure Batch**

* **Ubuntu Server (con controladores RDMA) para grupos de contenedores de Azure Batch**

Estas imágenes solo se admiten para usarlas en los grupos de Azure Batch. Cuentan con:

* Un tiempo de ejecución de contenedor [Moby](https://github.com/moby/moby) instalado previamente 

* Controladores de GPU NVIDIA instalados previamente para optimizar la implementación en máquinas virtuales de la serie N de Azure

* Imágenes con o sin controladores RDMA instalados previamente. Estos controladores permiten que los nodos de grupo accedan a la red RDMA de Azure cuando se implementan en tamaños de VM compatibles con RDMA  

También puede crear imágenes personalizadas a partir de máquinas virtuales que ejecutan Docker en una de las distribuciones Linux compatibles con Batch. Si elige proporcionar su propia imagen personalizada de Linux, consulte las instrucciones de [Uso de una imagen personalizada administrada para crear un grupo de máquinas virtuales](batch-custom-images.md).

Para la compatibilidad de una imagen personalizada con Docker, instale [Docker Community Edition (CE)](https://www.docker.com/community-edition) o [Docker Enterprise Edition (EE)](https://www.docker.com/enterprise-edition).

Consideraciones adicionales para usar una imagen Linux personalizada:

* Para aprovechar el rendimiento de GPU de los tamaños de la serie N de Azure al usar una imagen personalizada, instale previamente los controladores NVIDIA. Además, debe instalar la utilidad de motor de Docker para GPU NVIDIA, [NVIDIA Docker](https://github.com/NVIDIA/nvidia-docker).

* Use un tamaño de máquina virtual compatible con RDAM para acceder a la red RDMA de Azure. Los controladores RDMA necesarios están instalados en HPC de CentOS e imágenes de Ubuntu compatibles con Batch. Puede ser necesaria una configuración adicional para ejecutar cargas de trabajo MPI. Consulte [Uso de instancias compatibles con RDMA o habilitadas para GPU en grupos de Batch](batch-pool-compute-intensive-sizes.md).


## <a name="container-configuration-for-batch-pool"></a>Configuración del contenedor de grupo de Batch

Para habilitar un grupo de Batch para ejecutar cargas de trabajo de contenedor, se debe especificar la opción [ContainerConfiguration](/dotnet/api/microsoft.azure.batch.containerconfiguration) en el objeto [VirtualMachineConfiguration](/dotnet/api/microsoft.azure.batch.virtualmachineconfiguration) del grupo. (Este artículo incluye vínculos a la referencia de la API de .NET de Batch. Puede encontrar la configuración correspondiente en la API [Batch Python](/python/api/azure.batch)).

Puede crear un grupo habilitado para contenedores con o sin imágenes de contenedor capturadas previamente, tal como se muestra en los ejemplos siguientes. El proceso de extracción (o captura previa) permite cargar previamente imágenes de contenedor desde Docker Hub u otro registro de contenedor en Internet. Para obtener el mejor rendimiento, use un [registro de contenedor de Azure](../container-registry/container-registry-intro.md) en la misma región que la cuenta de Batch.

La ventaja de capturar previamente las imágenes de contenedor es que, cuando las tareas comienzan a ejecutarse por primera vez, no tienen que esperar a que la imagen del contenedor se descargue. La configuración de contenedor extrae las imágenes de contenedor de las máquinas virtuales cuando se crea el grupo. Las tareas que se ejecutan en el grupo pueden entonces hacer referencia a la lista de las imágenes de contenedor y opciones de ejecución de contenedor.


### <a name="pool-without-prefetched-container-images"></a>Grupo sin imágenes de contenedor previamente capturadas

Para configurar el grupo habilitado para contenedores sin imágenes de contenedor previamente capturadas, defina los objetos `ContainerConfiguration` y `VirtualMachineConfiguration` como se muestra en el ejemplo de Python siguiente. En este ejemplo se usa la imagen de Ubuntu Server para grupos de contenedores de Azure Batch de Marketplace.


```python
image_ref_to_use = batch.models.ImageReference(
        publisher='microsoft-azure-batch',
        offer='ubuntu-server-container',
        sku='16-04-lts',
        version='latest')

"""
Specify container configuration. This is required even though there are no prefetched images.
"""

container_conf = batch.models.ContainerConfiguration()

new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batch.models.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            container_configuration=container_conf,
            node_agent_sku_id='batch.node.ubuntu 16.04'),
        vm_size='STANDARD_D1_V2',
        target_dedicated_nodes=1)
...
```


### <a name="prefetch-images-for-container-configuration"></a>Captura previa de imágenes para la configuración de contenedor

Para la captura previa de imágenes de contenedor en el grupo, agregue la lista de imágenes de contenedor (`container_image_names`, en Python) a `ContainerConfiguration`. 

En el siguiente ejemplo básico de Python se muestra cómo capturar previamente una imagen de contenedor Ubuntu estándar desde [Docker Hub](https://hub.docker.com).

```python
image_ref_to_use = batch.models.ImageReference(
    publisher='microsoft-azure-batch',
    offer='ubuntu-server-container',
    sku='16-04-lts',
    version='latest')

"""
Specify container configuration, fetching the official Ubuntu container image from Docker Hub. 
"""

container_conf = batch.models.ContainerConfiguration(container_image_names=['ubuntu'])

new_pool = batch.models.PoolAddParameter(
    id=pool_id,
    virtual_machine_configuration=batch.models.VirtualMachineConfiguration(
        image_reference=image_ref_to_use,
        container_configuration=container_conf,
        node_agent_sku_id='batch.node.ubuntu 16.04'),
    vm_size='STANDARD_D1_V2',
    target_dedicated_nodes=1)
...
```


En el siguiente ejemplo de C# se da por hecho que quiere capturar previamente una imagen de TensorFlow desde [Docker Hub](https://hub.docker.com). Este ejemplo contiene una tarea de inicio que se ejecuta en el host de máquina virtual de los nodos de grupo. Podría ejecutar una tarea de inicio en el host, por ejemplo, para montar un servidor de archivos al que pueda accederse desde los contenedores.

```csharp

ImageReference imageReference = new ImageReference(
    publisher: "microsoft-azure-batch",
    offer: "ubuntu-server-container",
    sku: "16-04-lts",
    version: "latest");

// Specify container configuration, prefetching Docker images
ContainerConfiguration containerConfig = new ContainerConfiguration(
    containerImageNames: new List<string> { "tensorflow/tensorflow:latest-gpu" } );

// VM configuration
VirtualMachineConfiguration virtualMachineConfiguration = new VirtualMachineConfiguration(
    imageReference: imageReference,
    containerConfiguration: containerConfig,
    nodeAgentSkuId: "batch.node.ubuntu 16.04");

// Set a native host command line start task
StartTask startTaskNative = new StartTask( CommandLine: "<native-host-command-line>" );

// Create pool
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 4,
    virtualMachineSize: "Standard_NC6",
    virtualMachineConfiguration: virtualMachineConfiguration, startTaskContainer);
...
```


### <a name="prefetch-images-from-a-private-container-registry"></a>Captura previa de imágenes desde un registro de contenedor privado

También puede capturar previamente imágenes de contenedor mediante la autenticación a un servidor de registro de contenedor privado. En el ejemplo siguiente, los objetos `ContainerConfiguration` y `VirtualMachineConfiguration` capturan previamente una imagen privada de TensorFlow de un registro de contenedor de Azure privado. La referencia de imagen es la misma que en el ejemplo anterior.

```csharp
// Specify a container registry
ContainerRegistry containerRegistry = new ContainerRegistry (
    registryServer: "myContainerRegistry.azurecr.io",
    username: "myUserName",
    password: "myPassword");

// Create container configuration, prefetching Docker images from the container registry
ContainerConfiguration containerConfig = new ContainerConfiguration(
    containerImageNames: new List<string> {
        "myContainerRegistry.azurecr.io/tensorflow/tensorflow:latest-gpu" },
    containerRegistries: new List<ContainerRegistry> { containerRegistry } );

// VM configuration
VirtualMachineConfiguration virtualMachineConfiguration = new VirtualMachineConfiguration(
    imageReference: imageReference,
    containerConfiguration: containerConfig,
    nodeAgentSkuId: "batch.node.ubuntu 16.04");

// Create pool
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    targetDedicatedComputeNodes: 4,
    virtualMachineSize: "Standard_NC6",
    virtualMachineConfiguration: virtualMachineConfiguration);
...
```


## <a name="container-settings-for-the-task"></a>Configuración de contenedor para la tarea

Para ejecutar tareas de contenedor en los nodos de proceso, debe especificar opciones concretas de contenedor, como las opciones de ejecución de tareas, las imágenes que se van a utilizar y el registro.

Use la propiedad `ContainerSettings` de las clases de tarea para configurar opciones específicas de contenedor. Esta configuración se define por la clase [TaskContainerSettings](/dotnet/api/microsoft.azure.batch.taskcontainersettings).

Si ejecuta tareas en imágenes de contenedor, la [tarea en la nube](/dotnet/api/microsoft.azure.batch.cloudtask) y la [tarea del administrador de trabajos](/dotnet/api/microsoft.azure.batch.cloudjob.jobmanagertask) requieren la configuración del contenedor. Sin embargo, la [tarea de inicio](/dotnet/api/microsoft.azure.batch.starttask), la [tarea de preparación de trabajos](/dotnet/api/microsoft.azure.batch.cloudjob.jobpreparationtask) y la [tarea de liberación de trabajos](/dotnet/api/microsoft.azure.batch.cloudjob.jobreleasetask) no requieren configuración del contenedor (es decir, pueden ejecutarse en un contexto de contenedor o directamente en el nodo).

Los elementos opcionales [ContainerRunOptions](/dotnet/api/microsoft.azure.batch.taskcontainersettings.containerrunoptions) son argumentos adicionales al comando `docker create` que ejecuta la tarea para crear el contenedor.

### <a name="container-task-working-directory"></a>Directorio de trabajo de tareas de contenedor

La línea de comandos de una tarea de contenedor de Azure Batch se ejecuta en un directorio de trabajo del contenedor muy similar al entorno que Batch configura para una tarea normal (que no es de tipo contenedor):

* Todos los directorios incluidos de forma recursiva debajo de `AZ_BATCH_NODE_ROOT_DIR` (la raíz de los directorios de Azure Batch en el nodo) se asignan al contenedor.
* Todas las variables de entorno de tareas se asignan al contenedor.
* El directorio de trabajo de la aplicación establecido es el mismo que en el caso de una tarea normal, por lo que puede usar características como paquetes de aplicación y archivos de recursos.

Dado que Batch cambia el directorio de trabajo predeterminado en el contenedor, la tarea se ejecuta en una ubicación diferente a la del directorio de trabajo habitual del contenedor (por ejemplo, `c:\` de forma predeterminada en un contenedor de Windows, o `/` en Linux, u otro directorio si lo ha configurado en la imagen de contenedor). Para asegurarse de que sus aplicaciones de contenedor se ejecutan correctamente en el contexto de Batch, realice una de las siguientes acciones: 

* Asegúrese de que la línea de comandos de la tarea (o el directorio de trabajo del contenedor) especifique una ruta de acceso absoluta, si no está ya configurada de esa manera.

* En ContainerSettings de la tarea, establezca un directorio de trabajo en las opciones de ejecución del contenedor. Por ejemplo, `--workdir /app`.

En el siguiente fragmento de código de Python se muestra una línea de comandos básica que se ejecuta en un contenedor de Ubuntu extraído de Docker Hub. En este caso, la opción de ejecución del contenedor `--rm` quita el contenedor después de que finaliza la tarea.

```python
task_id = 'sampletask'
task_container_settings = batch.models.TaskContainerSettings(
    image_name='ubuntu', 
    container_run_options='--rm')
task = batch.models.TaskAddParameter(
    id=task_id,
    command_line='/bin/echo hello',
    container_settings=task_container_settings
)

```

En el siguiente ejemplo de C# muestra la configuración de contenedor básica para una tarea en la nube:

```csharp
// Simple container task command

string cmdLine = "c:\myApp.exe";

TaskContainerSettings cmdContainerSettings = new TaskContainerSettings (
    imageName: "tensorflow/tensorflow:latest-gpu",
    containerRunOptions: "--rm --read-only"
    );

CloudTask containerTask = new CloudTask (
    id: "Task1",
    containerSettings: cmdContainerSettings,
    commandLine: cmdLine); 
```


## <a name="next-steps"></a>Pasos siguientes

* Consulte también el kit de herramientas [Batch Shipyard](https://github.com/Azure/batch-shipyard) para facilitar la implementación de las cargas de trabajo del elemento contenedor en Azure Batch a través de la receta [Shipyard](https://github.com/Azure/batch-shipyard/tree/master/recipes).

* Para más información sobre cómo instalar y usar Docker CE en Linux, consulte la documentación de [Docker](https://docs.docker.com/engine/installation/).

* Para más información sobre imágenes personalizadas, consulte [Uso de una imagen personalizada administrada para crear un grupo de máquinas virtuales](batch-custom-images.md).

* Más información sobre el [proyecto Moby](https://mobyproject.org/), un marco para crear sistemas basados en contenedor.