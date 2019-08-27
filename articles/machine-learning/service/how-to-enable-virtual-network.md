---
title: Protección de experimentos e inferencias en una red virtual
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo proteger los trabajos de experimentación o aprendizaje y los trabajos de inferencia o puntuación de Azure Machine Learning en una instancia de Azure Virtual Network.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 08/05/2019
ms.openlocfilehash: 3f13405a3660f8b2aaa053f82ed03d088fb4d733
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2019
ms.locfileid: "69897542"
---
# <a name="secure-azure-ml-experimentation-and-inference-jobs-within-an-azure-virtual-network"></a>Protección de los trabajos de experimentación e inferencia de ML en una instancia de Azure Virtual Network

En este artículo, obtendrá información sobre cómo proteger los trabajos de experimentación o aprendizaje y los trabajos de inferencia o puntuación de Azure Machine Learning en una instancia de Azure Virtual Network (red virtual). 

Una **red virtual** actúa como un límite de seguridad, aislando los recursos de Azure de la internet pública. También pude unir una red virtual de Azure a la red local. Mediante la unión de redes puede entrenar de forma segura sus modelos y acceder a los modelos implementados para la inferencia.

Azure Machine Learning Service depende de otros servicios de Azure para los recursos de proceso. Los recursos de proceso, o [destinos de proceso](concept-compute-target.md), se usan para entrenar e implementar modelos. Los destinos se pueden crear dentro de una red virtual. Por ejemplo, puede usar Microsoft Data Science Virtual Machine para entrenar un modelo y, después, implementarlo en Azure Kubernetes Service (AKS). Para más información acerca de las redes virtuales, consulte [Introducción a Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview).

En este artículo también se proporciona información detallada acerca de la *configuración de seguridad avanzada*, información que no es necesaria para casos de uso básicos o experimentales. Algunas secciones de este artículo proporcionan información acerca de la configuración de diversos escenarios. No es necesario completar las instrucciones en orden ni en su totalidad.

## <a name="prerequisites"></a>Requisitos previos

+ Un [área de trabajo](how-to-manage-workspace.md) de Azure Machine Learning Service. 

+ Conocimientos prácticos generales sobre el [servicio Azure Virtual Network](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview) y las [redes IP](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

+ Una red virtual y una subred preexistentes que se usarán con los recursos de proceso. 

## <a name="use-a-storage-account-for-your-workspace"></a>Uso de una cuenta de almacenamiento para el área de trabajo

Para usar una cuenta de Azure Storage para el área de trabajo en una red virtual, siga estos pasos:

1. Cree una instancia de un proceso de experimentación (por ejemplo, una instancia de Proceso de Machine Learning) detrás de una red virtual o vincule una instancia de un proceso de experimentación al área de trabajo (por ejemplo, un clúster de HDInsight o una máquina virtual). 

   Para más información, consulte las secciones "Uso de una instancia de Proceso de Machine Learning" y "Uso de una máquina virtual o un clúster de HDInsight" de este artículo.

1. En Azure Portal, vaya al almacenamiento que está vinculado al área de trabajo. 

   ![Almacenamiento asociado al área de trabajo de Azure Machine Learning Service](./media/how-to-enable-virtual-network/workspace-storage.png)

1. En la página de **Azure Storage**, seleccione __Firewalls y redes virtuales__. 

   ![El área "Firewalls y redes virtuales" de la página de Azure Storage de Azure Portal](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks.png)

1. En la página __Firewalls y redes virtuales__, realice estas acciones:
    - Seleccione __Redes seleccionadas__.
    - En __Redes virtuales__, seleccione el vínculo __Agregar red virtual existente__. Esta acción agrega la red virtual en la que reside la instancia del proceso de experimentación (consulte el paso 1).
    - Seleccione la casilla __Permitir que los servicios de Microsoft de confianza accedan a esta cuenta de almacenamiento__.

    > [!IMPORTANT]
    > Al trabajar con el SDK de Azure Machine Learning, el entorno de desarrollo debe poder conectarse a la cuenta de Azure Storage. Si la cuenta de almacenamiento se encuentra dentro de una red virtual, el firewall debe permitir el acceso desde la dirección IP del entorno de desarrollo.
    >
    > Para habilitar el acceso a la cuenta de almacenamiento, visite __Firewalls y redes virtuales__ de la cuenta de almacenamiento *desde un explorador web en el cliente de desarrollo*. A continuación, use la casilla __Agregar la dirección IP del cliente__ para agregar la dirección IP del cliente al campo __INTERVALO DE DIRECCIONES__. También puede usar el campo __INTERVALO DE DIRECCIONES__ para especificar manualmente la dirección IP del entorno de desarrollo. Una vez agregada la dirección IP del cliente, este puede acceder a la cuenta de almacenamiento mediante el SDK.

   ![Panel "Firewalls y redes virtuales" de Azure Portal](./media/how-to-enable-virtual-network/storage-firewalls-and-virtual-networks-page.png)

1. Al ejecutar el experimento, en el código de la experimentación, cambie la configuración de ejecución para usar Azure Blob Storage:

    ```python
    run_config.source_directory_data_store = "workspaceblobstore"
    ```

> [!IMPORTANT]
> La _cuenta de almacenamiento predeterminada_ para Azure Machine Learning Service se puede colocar en una red virtual _solo para la realización de experimentos_.
>
> Las _cuentas de almacenamiento no predeterminadas_  se pueden colocar en una red virtual _solo para la realización de experimentos_.
>
> Tanto las cuentas de almacenamiento predeterminadas como las no predeterminadas que se usan para la _inferencia_ deben tener _acceso sin restricciones a la cuenta de almacenamiento_.
>
> Si no está seguro de si ha modificado la configuración, consulte la sección "Modificación de la regla de acceso de red predeterminada" del artículo [Configuración de redes virtuales y firewalls de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-network-security). Siga las instrucciones para permitir el acceso desde todas las redes durante la inferencia, o puntuación de modelos.

## <a name="use-a-key-vault-instance-with-your-workspace"></a>Uso de una instancia de Key Vault con el área de trabajo

Azure Machine Learning Service usa la instancia de Key Vault que está asociada al área de trabajo para almacenar las siguientes credenciales:
* La cadena de conexión de cuenta de almacenamiento asociada
* Contraseñas para las instancias de Azure Container Repository
* Cadenas de conexión a almacenes de datos

Para usar las funcionalidades de experimentación de Azure Machine Learning con Azure Key Vault detrás de una red virtual, siga estos pasos:
1. Vaya a la instancia de Key Vault asociada al área de trabajo. 

   ![Instancia de Key Vault asociada al área de trabajo de Azure Machine Learning Service](./media/how-to-enable-virtual-network/workspace-key-vault.png)

1. En la **página de Key Vault**, en el panel izquierdo, seleccione __Firewalls y redes virtuales__. 

   ![Sección "Firewalls y redes virtuales" del panel de Key Vault](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks.png)

1. En la página __Firewalls y redes virtuales__, realice estas acciones:
    - Haga clic en __Redes seleccionadas__ en __Permitir el acceso desde__.
    - En __Redes virtuales__, seleccione __Agregar redes virtuales existentes__ para agregar la red virtual en que reside el proceso de experimentación.
    - En __¿Quiere permitir que los servicios de confianza de Microsoft puedan omitir este firewall?__ , seleccione __Sí__.

   ![Sección "Firewalls y redes virtuales" del panel de Key Vault](./media/how-to-enable-virtual-network/key-vault-firewalls-and-virtual-networks-page.png)

## <a name="use-a-machine-learning-compute-instance"></a>Uso de una instancia de Proceso de Machine Learning

Para usar una instancia de Proceso de Machine Learning en una red virtual, tenga en cuenta los siguientes requisitos de red:

- La red virtual debe estar en la misma suscripción y región que el área de trabajo de Azure Machine Learning Service.

- La subred que se especifique para el clúster de proceso debe tener suficientes direcciones IP sin asignar para alojar el número de máquinas virtuales que estén destinadas al clúster. Si la subred no tiene suficientes direcciones IP sin asignar, el clúster se asignará parcialmente.

- Si va a proteger la red virtual mediante la restricción del tráfico, deje abiertos algunos puertos para el servicio de proceso. Para más información, vea la sección [Puertos obligatorios](#mlcports).

- Compruebe si sus directivas de seguridad o bloqueos del grupo de recursos o la suscripción de la red virtual restringen los permisos para administrar las redes virtuales.

- Si va a colocar varios clústeres de proceso en una red virtual, es posible que tenga que solicitar un aumento de la cuota para uno o varios de los recursos.

    La instancia de Proceso de Machine Learning asigna automáticamente recursos de red adicionales al grupo de recursos que contiene la red virtual. Para cada clúster de proceso, el servicio asigna los recursos siguientes:

    - Un grupo de seguridad de red

    - Una dirección IP pública

    - Un equilibrador de carga

  Estos recursos están limitados por las [cuotas de recursos](https://docs.microsoft.com/azure/azure-subscription-service-limits) de la suscripción.

### <a id="mlcports"></a> Puertos necesarios

El Proceso de Machine Learning usa actualmente el servicio Azure Batch para aprovisionar máquinas virtuales en la red virtual especificada. La subred debe permitir las comunicaciones entrantes desde el servicio Batch. Use esta comunicación para programar ejecuciones en los nodos de Proceso de Machine Learning y para comunicarse tanto con Azure Storage como otros recursos. El servicio Batch agrega grupos de seguridad de red (NSG) en el nivel de las interfaces de red (NIC) que están asociadas a las máquinas virtuales. Estos grupos de seguridad de red configuran automáticamente las reglas de entrada y salida para permitir el siguiente tráfico:

- Tráfico de entrada TCP en los puertos 29876 y 29877 desde una __etiqueta de servicio__ __BatchNodeManagement__.

    ![Una regla de entrada que usa la etiqueta de servicio BatchNodeManagement](./media/how-to-enable-virtual-network/batchnodemanagement-service-tag.png)

- (Opcional) Tráfico TCP de entrada en el puerto 22 para permitir el acceso remoto. Use este puerto solo si desea conectarse mediante SSH en la IP pública.

- Tráfico saliente en cualquier puerto para la red virtual.

- Tráfico saliente en cualquier puerto para Internet.

Tenga cuidado si modifica reglas de entrada o salida en los grupos de seguridad de red configurados para Batch o las agrega a ellos. Si un grupo de seguridad de red bloquea la comunicación con los nodos de ejecución, el servicio de proceso establece el estado de los nodos de ejecución en inutilizable.

No es necesario especificar grupos de seguridad de red en el nivel de subred, porque el servicio Azure Batch configura los suyos propios. Sin embargo, si la subred especificada tiene asociados grupos de seguridad de red o un firewall, configure las reglas de seguridad de entrada y salida como se ha explicado antes.

La configuración de la regla de NSG en Azure Portal se muestra en las siguientes imágenes:

![Reglas de NSG de entrada para Proceso de Machine Learning](./media/how-to-enable-virtual-network/amlcompute-virtual-network-inbound.png)

![Reglas de NSG de salida para Proceso de Machine Learning](./media/how-to-enable-virtual-network/experimentation-virtual-network-outbound.png)

### <a id="limiting-outbound-from-vnet"></a> Limitación de la conectividad saliente de la red virtual

Si no desea usar las reglas de salida predeterminadas y desea limitar el acceso de salida de la red virtual, siga estos pasos:

- Deniegue la conexión saliente a Internet mediante las reglas de NSG.

- Limite el tráfico de salida a lo siguiente:
   - Azure Storage, mediante la __etiqueta de servicio__ de __Storage.Region_Name__ (por ejemplo, Storage.EastUS)
   - Azure Container Registry, mediante la __etiqueta de servicio__ de __AzureContainerRegistry.Region_Name__ (por ejemplo, AzureContainerRegistry.EastUS)
   - Azure Machine Learning Service, mediante la __etiqueta de servicio__ de __AzureMachineLearning__

La configuración de la regla de NSG en Azure Portal se muestra en la siguiente imagen:

![Reglas de NSG de salida para Proceso de Machine Learning](./media/how-to-enable-virtual-network/limited-outbound-nsg-exp.png)

### <a name="user-defined-routes-for-forced-tunneling"></a>Rutas definidas por el usuario para la tunelización forzada

Si usa la tunelización forzada con Proceso de Machine Learning, agregue [rutas definidas por el usuario (UDR)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview) a la subred que contiene el recurso del proceso.

* Establezca una ruta definida por el usuario para cada dirección IP que use el servicio Azure Batch en la región en la que existen sus recursos. Estas UDR permiten que el servicio Batch se comunique con los nodos de proceso para programar tareas. Para obtener la lista de direcciones IP del servicio Batch, utilice uno de los métodos siguientes:

    * Descargue los [intervalos de direcciones IP y las etiquetas de servicio de Azure](https://www.microsoft.com/download/details.aspx?id=56519) y busque `BatchNodeManagement.<region>` en el archivo, donde `<region>` es su región de Azure.

    * Use la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) para descargar la información. En el ejemplo siguiente se descarga la información de la dirección IP, que se filtra para la región Este de EE. UU. 2:

        ```azurecli-interactive
        az network list-service-tags -l "East US 2" --query "values[?starts_with(id, 'Batch')] | [?properties.region=='eastus2']"
        ```

* El tráfico de salida hacia Azure Storage no debe bloquearlo el dispositivo de red local. En concreto, las direcciones URL tienen el formato `<account>.table.core.windows.net`, `<account>.queue.core.windows.net` y `<account>.blob.core.windows.net`.

Cuando agregue las rutas definidas por el usuario, defina la ruta del prefijo de cada dirección IP de Batch relacionada y en __Tipo del próximo salto__, seleccione __Internet__. En la imagen siguiente se muestra un ejemplo de esta UDR en Azure Portal:

![Ejemplo de una ruta definida por el usuario para un prefijo de dirección](./media/how-to-enable-virtual-network/user-defined-route.png)

Para más información, consulte [Creación de un grupo de Azure Batch en una red virtual](../../batch/batch-virtual-network.md#user-defined-routes-for-forced-tunneling).

### <a name="create-a-machine-learning-compute-cluster-in-a-virtual-network"></a>Creación de un clúster de Proceso de Machine Learning en una red virtual

Para crear un clúster de Proceso de Machine Learning, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com), seleccione el área de trabajo de Azure Machine Learning Service.

1. En la sección __Aplicación__, seleccione __Proceso__, y, después, __Agregar proceso__.

1. Para configurar este recurso de proceso para que use una red virtual, siga estos pasos:

    a. En __Configuración de red__, seleccione __Opciones avanzadas__.

    b. En la lista desplegable __Grupo de recursos__, seleccione el grupo de recursos que contiene la red virtual.

    c. En la lista desplegable __Red virtual__, seleccione la red que contiene la subred.

    d. En la lista desplegable __Subred__, seleccione la subred que se va a usar.

   ![Configuración de la red virtual de Proceso de Machine Learning](./media/how-to-enable-virtual-network/amlcompute-virtual-network-screen.png)

También puede crear un clúster de Proceso de Machine Learning con el SDK de Azure Machine Learning. El código siguiente crea un nuevo clúster de Proceso de Machine Learning en la `default` subred de una red virtual denominada `mynetwork`:

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# The Azure virtual network name, subnet, and resource group
vnet_name = 'mynetwork'
subnet_name = 'default'
vnet_resourcegroup_name = 'mygroup'

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print("Found existing cpucluster")
except ComputeTargetException:
    print("Creating new cpucluster")

    # Specify the configuration for the new cluster
    compute_config = AmlCompute.provisioning_configuration(vm_size="STANDARD_D2_V2",
                                                           min_nodes=0,
                                                           max_nodes=4,
                                                           vnet_resourcegroup_name=vnet_resourcegroup_name,
                                                           vnet_name=vnet_name,
                                                           subnet_name=subnet_name)

    # Create the cluster with the specified name and configuration
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

    # Wait for the cluster to be completed, show the output log
    cpu_cluster.wait_for_completion(show_output=True)
```

Cuando finaliza el proceso de creación, el modelo se entrena mediante el clúster en un experimento. Para más información, consulte [Selección y uso de un destino de proceso para entrenamiento](how-to-set-up-training-targets.md).

## <a name="use-a-virtual-machine-or-hdinsight-cluster"></a>Uso de una máquina virtual o un clúster de HDInsight

> [!IMPORTANT]
> Azure Machine Learning Service solo admite máquinas virtuales que ejecuten Ubuntu.

Para usar una máquina virtual o un clúster de Azure HDInsight en una red virtual con su área de trabajo, realice las siguientes operaciones:

1. Cree una máquina virtual o un clúster de HDInsight mediante Azure Portal o la CLI de Azure y colóquelo en una red virtual de Azure. Para más información, consulte los siguientes artículos.
    * [Creación y administración de redes virtuales de Azure para máquinas virtuales Linux](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)

    * [Extender HDInsight mediante una red virtual de Azure](https://docs.microsoft.com/azure/hdinsight/hdinsight-extend-hadoop-virtual-network)

1. Para permitir que Azure Machine Learning Service se comunique con el puerto SSH en la máquina virtual o el clúster, configure una entrada de origen para el grupo de seguridad de red. El puerto SSH suele ser el 22. Para permitir el tráfico desde este origen, siga estos pasos:

    * En la lista desplegable __Origen__, seleccione __Etiqueta de servicio__.

    * En la lista desplegable __Etiqueta de servicio de origen__ , seleccione __AzureMachineLearning__.

    * En la lista desplegable __Intervalos de puertos de origen__, seleccione __*__ .

    * En la lista desplegable __Destino__, seleccione __Cualquiera__.

    * En la lista desplegable __Intervalos de puertos de destino__, seleccione __22__.

    * En __Protocolo__, seleccione __Cualquiera__.

    * En __Acción__, seleccione __Permitir__.

   ![Reglas de entrada para realizar la experimentación en una máquina virtual o un clúster de HDInsight dentro de una red virtual](./media/how-to-enable-virtual-network/experimentation-virtual-network-inbound.png)

    Conserve las reglas de salida predeterminadas para el grupo de seguridad de red. Para más información, consulte las reglas de seguridad predeterminadas en [Grupos de seguridad](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules).

    Si no desea usar las reglas de salida predeterminadas, pero desea limitar el acceso de salida de la red virtual, consulte la sección [Limitación de la conectividad saliente de la red virtual](#limiting-outbound-from-vnet).

1. Conecte el clúster de HDInsight o de máquina virtual a su área de trabajo de Azure Machine Learning Service. Para más información, consulte [Configuración de destinos de proceso del entrenamiento del modelo](how-to-set-up-training-targets.md).

<a id="aksvnet"></a>

## <a name="use-azure-kubernetes-service-aks"></a>Uso de Azure Kubernetes Service (AKS)

Para agregar Azure Kubernetes Service de una red virtual a su área de trabajo, siga estos pasos:

> [!IMPORTANT]
> Antes de comenzar el siguiente procedimiento, compruebe los requisitos previos y planee el direccionamiento IP del clúster. Para más información, consulte [Configuración de redes avanzadas en Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/configure-advanced-networking).
>
> La instancia de AKS y la instancia de Azure Virtual Network deben estar en la misma región.

1. En [Azure Portal](https://portal.azure.com), asegúrese de que el grupo de seguridad de red que controla la red virtual tiene una regla de entrada que se ha habilitado para Azure Machine Learning Service mediante __AzureMachineLearning__ como **SOURCE**.

    ![Panel Agregar proceso de Azure Machine Learning Service](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-aml.png)

1. Seleccione el área de trabajo de Azure Machine Learning Service.

1. En la sección __Aplicación__, seleccione __Proceso__, y, después, __Agregar proceso__.

1. Para configurar este recurso de proceso para que use una red virtual, siga estos pasos:

    - En __Configuración de red__, seleccione __Opciones avanzadas__.

    - En la lista desplegable __Grupo de recursos__, seleccione el grupo de recursos que contiene la red virtual.

    - En la lista desplegable __Red virtual__, seleccione la red que contiene la subred.

    - En la lista desplegable __Subred__, seleccione la subred.

    - En el cuadro __Intervalo de direcciones del servicio Kubernetes__, escriba el intervalo de direcciones de servicio de Kubernetes. Este intervalo de direcciones utiliza un intervalo de IP de notación CIDR (enrutamiento entre dominios sin clases) para definir las direcciones IP que están disponibles para el clúster. No debe superponerse con ningún intervalo IP de subred (por ejemplo, 10.0.0.0/16).

    - En el cuadro __Dirección IP del servicio DNS de Kubernetes__, escriba la dirección IP del servicio DNS de Kubernetes. Esta dirección IP se asigna al servicio DNS de Kubernetes. Debe estar dentro del intervalo de direcciones del servicio Kubernetes (por ejemplo, 10.0.0.10).

    - En el cuadro __Dirección de puente de Docker__, escriba la dirección del puente de Docker. Esta dirección IP se asigna al puente de Docker. No debe estar en ningún intervalo IP de subred o en el intervalo de direcciones del servicio Kubernetes (por ejemplo, 172.17.0.1/16).

   ![Azure Machine Learning Service: Configuración de la red virtual del Proceso de Machine Learning](./media/how-to-enable-virtual-network/aks-virtual-network-screen.png)

1. Asegúrese de que el grupo de seguridad de red que controla la red virtual tiene una regla de seguridad de entrada habilitada para el punto de conexión de puntuación, de modo que se le pueda llamar desde fuera de la red virtual.
   > [!IMPORTANT]
   > Mantenga las reglas de salida predeterminadas para el grupo de seguridad de red. Para más información, consulte las reglas de seguridad predeterminadas en [Grupos de seguridad](https://docs.microsoft.com/azure/virtual-network/security-overview#default-security-rules).
  
   ![Regla de seguridad de entrada](./media/how-to-enable-virtual-network/aks-vnet-inbound-nsg-scoring.png)

También puede usar el SDK de Azure Machine Learning para agregar Azure Kubernetes Service en una red virtual. Si ya tiene un clúster de AKS en una red virtual, asócielo al área de trabajo como se describe en el tema sobre la [implementación en AKS](how-to-deploy-to-aks.md). El código siguiente crea una instancia de Azure Kubernetes Service en la subred `default` de una red virtual denominada `mynetwork`:

```python
from azureml.core.compute import ComputeTarget, AksCompute

# Create the compute configuration and set virtual network information
config = AksCompute.provisioning_configuration(location="eastus2")
config.vnet_resourcegroup_name = "mygroup"
config.vnet_name = "mynetwork"
config.subnet_name = "default"
config.service_cidr = "10.0.0.0/16"
config.dns_service_ip = "10.0.0.10"
config.docker_bridge_cidr = "172.17.0.1/16"

# Create the compute target
aks_target = ComputeTarget.create(workspace=ws,
                                  name="myaks",
                                  provisioning_configuration=config)
```

Cuando finalice el proceso de creación, puede ejecutar una inferencia, o puntuación de modelos, en un clúster de AKS detrás de una red virtual. Para más información, consulte [Implementación en AKS](how-to-deploy-to-aks.md).

## <a name="next-steps"></a>Pasos siguientes

* [Configuración de entornos de entrenamiento](how-to-set-up-training-targets.md)
* [Lugar de implementación de modelos](how-to-deploy-and-where.md)
* [Implementación segura de modelos con SSL](how-to-secure-web-service.md)

