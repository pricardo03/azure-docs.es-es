---
title: Usar varios grupos de nodos en Azure Kubernetes Service (AKS)
description: Obtenga información sobre cómo crear y administrar varios grupos de nodos para un clúster en Azure Kubernetes Service (AKS)
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: article
ms.date: 05/17/2019
ms.author: iainfou
ms.openlocfilehash: a295dfa1f7f2c58b3e45036212434837ac4bfb4d
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66475457"
---
# <a name="preview---create-and-manage-multiple-node-pools-for-a-cluster-in-azure-kubernetes-service-aks"></a>Obtener una vista previa: crear y administrar varios grupos de nodos para un clúster en Azure Kubernetes Service (AKS)

En Azure Kubernetes Service (AKS), los nodos de la misma configuración se agrupan en *grupos de nodos*. Estos grupos de nodos contienen las máquinas virtuales subyacentes que se ejecutan las aplicaciones. El número inicial de nodos y su tamaño (SKU) se define al crear un clúster AKS, que crea un *nodo grupo predeterminado*. Para admitir aplicaciones que tengan diferentes de proceso o demandas de almacenamiento, puede crear grupos de nodos adicionales. Por ejemplo, puede usar estos grupos de nodos adicionales proporcionan GPU para aplicaciones de proceso intensivo, o tener acceso a almacenamiento de SSD de alto rendimiento.

Este artículo muestra cómo crear y administrar varios grupos de nodos en un clúster de AKS. Esta funcionalidad actualmente está en su versión preliminar.

> [!IMPORTANT]
> Características de versión preliminar AKS son autoservicio, participación. Se proporcionan para recopilar comentarios y los errores de nuestra comunidad. En la vista previa, estas características no están diseñadas para su uso en producción. Características en versión preliminar pública se incluyen en el soporte técnico de "mejor esfuerzo". Asistencia de los equipos de soporte técnico de AKS está disponible durante el horario comercial del Pacífico (PST) solo timezone. Para obtener más información, consulte los siguientes artículos de soporte técnico:
>
> * [Directivas de soporte técnico AKS][aks-support-policies]
> * [Preguntas más frecuentes de soporte técnico de Azure][aks-faq]

## <a name="before-you-begin"></a>Antes de empezar

Necesita la CLI de Azure versión 2.0.61 o posterior instalado y configurado. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure][install-azure-cli].

### <a name="install-aks-preview-cli-extension"></a>Instalación de la extensión aks-preview de la CLI

Los comandos de CLI para crear y administrar varios grupos de nodos que están disponibles en el *-versión preliminar de aks* extensión de la CLI. Instalar el *-versión preliminar de aks* extensión de CLI de Azure mediante el [Agregar extensión az] [ az-extension-add] de comandos, tal como se muestra en el ejemplo siguiente:

```azurecli-interactive
az extension add --name aks-preview
```

> [!NOTE]
> Si ha instalado anteriormente el *-versión preliminar de aks* instalar haya disponible de extensión, actualizaciones utilizando el `az extension update --name aks-preview` comando.

### <a name="register-multiple-node-pool-feature-provider"></a>Registrar proveedor de características de grupo de varios nodos

Para crear un clúster de AKS que puede usar varios grupos de nodos, debe habilitar primero dos marcas de características en su suscripción. Clústeres de varios nodos grupo usan un conjunto de escalado de máquinas virtuales (VMSS) para administrar la implementación y configuración de los nodos de Kubernetes. Registrar el *MultiAgentpoolPreview* y *VMSSPreview* las marcas de características mediante el [register de la característica de az] [ az-feature-register] comando tal como se muestra en el ejemplo siguiente:

```azurecli-interactive
az feature register --name MultiAgentpoolPreview --namespace Microsoft.ContainerService
az feature register --name VMSSPreview --namespace Microsoft.ContainerService
```

> [!NOTE]
> Cualquier clúster de AKS crea después de que se ha registrado correctamente el *MultiAgentpoolPreview* usar esta experiencia de clúster de versión preliminar. Para seguir creando clústeres normales, totalmente compatible, no habilite características de vista previa en las suscripciones de producción. Utilice una prueba independiente o la suscripción de Azure de desarrollo para probar características de vista previa.

Tarda unos minutos en que el estado muestre *Registrado*. Puede comprobar el estado del registro con el comando [az feature list][az-feature-list]:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/MultiAgentpoolPreview')].{Name:name,State:properties.state}"
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/VMSSPreview')].{Name:name,State:properties.state}"
```

Cuando todo esté listo, actualice el registro del proveedor de recursos *Microsoft.ContainerService* con el comando [az provider register][az-provider-register]:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="limitations"></a>Limitaciones

Al crear y administrar clústeres AKS que admiten varios grupos de nodos, se aplican las siguientes limitaciones:

* Varios grupos de nodos solo están disponibles para los clústeres creados después de registrar correctamente el *MultiAgentpoolPreview* y *VMSSPreview* características para su suscripción. No se puede agregar o administrar grupos de nodos con un clúster de AKS existente creado antes de que estas características se han registrado correctamente.
* No se puede eliminar el primer grupo de nodos.
* No se puede usar el complemento de enrutamiento de aplicación HTTP.
* No puede utilizar una plantilla de Resource Manager existente al igual que con la mayoría de las operaciones de grupos de nodos de agregar/actualizar/eliminar. En su lugar, [usar una plantilla de Resource Manager independiente](#manage-node-pools-using-a-resource-manager-template) para realizar cambios en grupos de nodos en un clúster de AKS.
* No se puede usar el Escalador automático de clúster (actualmente en versión preliminar de AKS).

Aunque esta característica está en versión preliminar, se aplican las siguientes limitaciones adicionales:

* El clúster de AKS puede tener un máximo de ocho grupos de nodos.
* El clúster de AKS puede tener un máximo de 400 nodos entre esos grupos de ocho nodos.

## <a name="create-an-aks-cluster"></a>Creación de un clúster de AKS

Para empezar, cree un clúster de AKS con un grupo de nodo único. En el ejemplo siguiente se usa el [crear grupo az] [ az-group-create] comando para crear un grupo de recursos denominado *myResourceGroup* en el *eastus* región. Un clúster de AKS denominado *myAKSCluster* , a continuación, se crea utilizando el [crear az aks] [ az-aks-create] comando. Un *--kubernetes versión* de *1.12.6* se usa para mostrar cómo actualizar un grupo de nodos en el paso siguiente. Puede especificar cualquier [admite la versión de Kubernetes][supported-versions].

```azurecli-interactive
# Create a resource group in East US
az group create --name myResourceGroup --location eastus

# Create a basic single-node AKS cluster
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --enable-vmss \
    --node-count 1 \
    --generate-ssh-keys \
    --kubernetes-version 1.12.6
```

La operación de creación del clúster tarda unos minutos.

Cuando el clúster esté listo, utilice el [az aks get-credentials] [ az-aks-get-credentials] comando para obtener las credenciales del clúster para su uso con `kubectl`:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

## <a name="add-a-node-pool"></a>Agregar un grupo de nodos

El clúster creado en el paso anterior tiene un bloque de nodo único. Vamos a agregar un segundo grupo de nodo mediante la [Agregar grupo de nodos de az aks] [ az-aks-nodepool-add] comando. En el ejemplo siguiente se crea un grupo de nodos denominado *mynodepool* que se ejecuta *3* nodos:

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 3
```

Para ver el estado de los grupos de nodos, use el [lista de grupo de nodos de az aks] [ az-aks-nodepool-list] comando y especifique el nombre del clúster y de grupo de recursos:

```azurecli-interactive
az aks nodepool list --resource-group myResourceGroup --cluster-name myAKSCluster -o table
```

La siguiente salida de ejemplo que muestra *mynodepool* se ha creado correctamente con tres nodos en el grupo de nodos. Cuando se creó el clúster de AKS en el paso anterior, el valor predeterminado es *nodepool1* se creó con un número de nodos de *1*.

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup         VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  --------------------  ---------------
VirtualMachineScaleSets  3        110        mynodepool  1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
```

> [!TIP]
> Si no hay ningún *OrchestratorVersion* o *VmSize* se especifica al agregar un grupo de nodos, los nodos se crean según los valores predeterminados para el clúster de AKS. En este ejemplo, que era la versión de Kubernetes *1.12.6* y el tamaño de nodo de *Standard_DS2_v2*.

## <a name="upgrade-a-node-pool"></a>Actualizar un grupo de nodos

Cuando se creó el clúster de AKS en el primer paso, una `--kubernetes-version` de *1.12.6* se ha especificado. Vamos a actualizar el *mynodepool* en Kubernetes *1.12.7*. Use la [actualización del grupo az aks nodo] [ az-aks-nodepool-upgrade] comando para actualizar el grupo de nodos, tal como se muestra en el ejemplo siguiente:

```azurecli-interactive
az aks nodepool upgrade \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --kubernetes-version 1.12.7 \
    --no-wait
```

Enumera el estado de los grupos de nodos utilizando de nuevo el [lista de grupo de nodos de az aks] [ az-aks-nodepool-list] comando. El ejemplo siguiente muestra que *mynodepool* está en el *Upgrading* estado *1.12.7*:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup    VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  ---------------  ---------------
VirtualMachineScaleSets  3        110        mynodepool  1.12.7                 100             Linux     Upgrading            myResourceGroup  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.12.6                 100             Linux     Succeeded            myResourceGroup  Standard_DS2_v2
```

Tarda unos minutos en actualizar los nodos a la versión especificada.

Como práctica recomendada, debe actualizar todos los grupos de nodos en un clúster de AKS a la misma versión de Kubernetes. La capacidad para actualizar los grupos de nodos individuales le permite realizar una actualización gradual y programar los pods entre grupos de nodos para mantener el tiempo de actividad de la aplicación.

## <a name="scale-a-node-pool"></a>Escalar un grupo de nodos

Como la aplicación cambiar las necesidades de carga de trabajo, es posible que deba escalar el número de nodos en un grupo de nodos. El número de nodos se puede escalar o reducir verticalmente.

<!--If you scale down, nodes are carefully [cordoned and drained][kubernetes-drain] to minimize disruption to running applications.-->

Para escalar el número de nodos en un grupo de nodos, use el [escalado del grupo de nodos de az aks] [ az-aks-nodepool-scale] comando. En el ejemplo siguiente se escala el número de nodos en *mynodepool* a *5*:

```azurecli-interactive
az aks nodepool scale \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-count 5 \
    --no-wait
```

Enumera el estado de los grupos de nodos utilizando de nuevo el [lista de grupo de nodos de az aks] [ az-aks-nodepool-list] comando. El ejemplo siguiente muestra que *mynodepool* está en el *escalado* estado con el nuevo recuento de *5* nodos:

```console
$ az aks nodepool list -g myResourceGroupPools --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup         VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  --------------------  ---------------
VirtualMachineScaleSets  5        110        mynodepool  1.12.7                 100             Linux     Scaling              myResourceGroupPools  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
```

Tarda unos minutos para completar la operación de escalado.

## <a name="delete-a-node-pool"></a>Eliminar un grupo de nodos

Si ya no necesita un grupo, puede eliminarlo y quitar los nodos de máquina virtual subyacentes. Para eliminar un grupo de nodos, use el [eliminar grupo de nodos de az aks] [ az-aks-nodepool-delete] comando y especifique el nombre del grupo de nodos. En el ejemplo siguiente se elimina el *mynoodepool* creado en los pasos anteriores:

> [!CAUTION]
> No hay ninguna opción de recuperación de pérdida de datos que se pueden producir cuando se elimina un grupo de nodos. Si no se puede programar los pods en otros grupos de nodos, esas aplicaciones no están disponibles. Asegúrese de que no elimine un grupo de nodos cuando las aplicaciones en uso no tienen copias de seguridad de datos o la capacidad para ejecutarse en otros grupos de nodos del clúster.

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster --name mynodepool --no-wait
```

La siguiente salida de ejemplo de la [lista de grupo de nodos de az aks] [ az-aks-nodepool-list] comando muestra que *mynodepool* está en el *eliminando* estado:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name        OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup         VmSize
-----------------------  -------  ---------  ----------  ---------------------  --------------  --------  -------------------  --------------------  ---------------
VirtualMachineScaleSets  5        110        mynodepool  1.12.7                 100             Linux     Deleting             myResourceGroupPools  Standard_DS2_v2
VirtualMachineScaleSets  1        110        nodepool1   1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
```

Tarda unos minutos en eliminar los nodos y el grupo de nodos.

## <a name="specify-a-vm-size-for-a-node-pool"></a>Especifique un tamaño de máquina virtual para un grupo de nodos

En los ejemplos anteriores para crear un grupo de nodos, se usó un tamaño de máquina virtual predeterminada para los nodos creados en el clúster. Es un escenario más común para que crear grupos de nodos con diferentes tamaños de máquina virtual y capacidades. Por ejemplo, puede crear un grupo de nodos que contiene los nodos con grandes cantidades de memoria o CPU, o un grupo de nodos que proporciona compatibilidad con GPU. En el paso siguiente, se [usar taints y tolerations](#schedule-pods-using-taints-and-tolerations) para indicar que el programador de Kubernetes a cómo limitar el acceso a los pods que se pueden ejecutar en esos nodos.

En el ejemplo siguiente, cree un grupo de nodos basado en GPU que usa el *Standard_NC6* tamaño de máquina virtual. Estas máquinas virtuales cuentan con la tarjeta Tesla K80 de NVIDIA. Para obtener información sobre los tamaños de máquina virtual disponibles, consulte [tamaños de máquinas virtuales Linux en Azure][vm-sizes].

Crear un grupo de nodos mediante la [Agregar grupo de nodos de az aks] [ az-aks-nodepool-add] nuevo comando. Esta vez, especifique el nombre *gpunodepool*y usar el `--node-vm-size` parámetro para especificar el *Standard_NC6* tamaño:

```azurecli-interactive
az aks nodepool add \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name gpunodepool \
    --node-count 1 \
    --node-vm-size Standard_NC6 \
    --no-wait
```

La siguiente salida de ejemplo de la [lista de grupo de nodos de az aks] [ az-aks-nodepool-list] comando muestra que *gpunodepool* es *crear* nodos con el especificado *VmSize*:

```console
$ az aks nodepool list -g myResourceGroup --cluster-name myAKSCluster -o table

AgentPoolType            Count    MaxPods    Name         OrchestratorVersion    OsDiskSizeGb    OsType    ProvisioningState    ResourceGroup         VmSize
-----------------------  -------  ---------  -----------  ---------------------  --------------  --------  -------------------  --------------------  ---------------
VirtualMachineScaleSets  1        110        gpunodepool  1.12.6                 100             Linux     Creating             myResourceGroupPools  Standard_NC6
VirtualMachineScaleSets  1        110        nodepool1    1.12.6                 100             Linux     Succeeded            myResourceGroupPools  Standard_DS2_v2
```

Tarda unos minutos la *gpunodepool* se cree correctamente.

## <a name="schedule-pods-using-taints-and-tolerations"></a>Programar con taints y tolerations de pods

Ahora tiene dos grupos de nodos en el clúster, el grupo de nodos de forma predeterminada crea inicialmente y el grupo de nodos basado en GPU. Use la [kubectl obtener nodos] [ kubectl-get] comando para ver los nodos del clúster. La salida del ejemplo siguiente muestra un nodo en cada grupo de nodos:

```console
$ kubectl get nodes

NAME                                 STATUS   ROLES   AGE     VERSION
aks-gpunodepool-28993262-vmss000000  Ready    agent   4m22s   v1.12.6
aks-nodepool1-28993262-vmss000000    Ready    agent   115m    v1.12.6
```

El programador de Kubernetes puede utilizar taints y tolerations para limitar las cargas de trabajo que se pueden ejecutar en los nodos.

* Un valor **taint** se aplica a un nodo que indica que solo se pueden programar pods específicos en él.
* Luego se aplica un valor **toleration** a un pod que le permite *tolerar* el valor taint de un nodo.

Para obtener más información sobre cómo usar avanzadas características Kubernetes programada, consulte [procedimientos recomendados para las características avanzadas de programador en AKS][taints-tolerations]

En este ejemplo, aplicar un su sabor al nodo basado en GPU mediante el [kubectl su sabor nodo] [ kubectl-taint] comando. Especifique el nombre del nodo de la salida de la anterior basado en GPU `kubectl get nodes` comando. Se aplica el su sabor como un *clave: valor* y, a continuación, una opción de programación. En el ejemplo siguiente se usa el *sku = gpu* emparejar y define los pods en caso contrario, tiene la *NoSchedule* capacidad:

```console
kubectl taint node aks-gpunodepool-28993262-vmss000000 sku=gpu:NoSchedule
```

El siguiente manifiesto YAML de ejemplo básico usa una tolerancia para permitir que el programador de Kubernetes ejecutar un pod NGINX en el nodo basado en GPU. Para que ver un ejemplo más adecuado, pero mucho tiempo ejecutar un trabajo de Tensorflow en el conjunto de datos MNIST [GPU de uso para cargas de trabajo de proceso intensivo en AKS][gpu-cluster].

Cree un archivo denominado `gpu-toleration.yaml` y cópielo en el ejemplo siguiente de YAML:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - image: nginx:1.15.9
    name: mypod
    resources:
      requests:
        cpu: 100m
        memory: 128Mi
      limits:
        cpu: 1
        memory: 2G
  tolerations:
  - key: "sku"
    operator: "Equal"
    value: "gpu"
    effect: "NoSchedule"
```

Programar el pod mediante la `kubectl apply -f gpu-toleration.yaml` comando:

```console
kubectl apply -f gpu-toleration.yaml
```

Tarda unos segundos para programar el pod y extraiga la imagen NGINX. Use la [kubectl describe pod] [ kubectl-describe] comando para ver el estado de pod. El siguiente ejemplo reducido de salida muestra el *sku = gpu:NoSchedule* se aplica la tolerancia. En la sección eventos, el programador ha asignado el pod a la *aks-gpunodepool-28993262-vmss000000* basado en GPU de nodo:

```console
$ kubectl describe pod mypod

[...]
Tolerations:     node.kubernetes.io/not-ready:NoExecute for 300s
                 node.kubernetes.io/unreachable:NoExecute for 300s
                 sku=gpu:NoSchedule
Events:
  Type    Reason     Age    From                                          Message
  ----    ------     ----   ----                                          -------
  Normal  Scheduled  4m48s  default-scheduler                             Successfully assigned default/mypod to aks-gpunodepool-28993262-vmss000000
  Normal  Pulling    4m47s  kubelet, aks-gpunodepool-28993262-vmss000000  pulling image "nginx:1.15.9"
  Normal  Pulled     4m43s  kubelet, aks-gpunodepool-28993262-vmss000000  Successfully pulled image "nginx:1.15.9"
  Normal  Created    4m40s  kubelet, aks-gpunodepool-28993262-vmss000000  Created container
  Normal  Started    4m40s  kubelet, aks-gpunodepool-28993262-vmss000000  Started container
```

Solo los pods que tienen este su sabor aplicado pueden programarse en nodos *gpunodepool*. Cualquier otro pod se programaría en el *nodepool1* grupo de nodos. Si crea grupos de nodos adicionales, puede usar taints adicionales y se pueden programar tolerations para limitar qué pods en esos recursos del nodo.

## <a name="manage-node-pools-using-a-resource-manager-template"></a>Administrar grupos de nodos mediante una plantilla de Resource Manager

Cuando usa una plantilla de Azure Resource Manager para crear y los recursos administrados, normalmente puede actualizar la configuración de la plantilla y volver a implementar para actualizar el recurso. Con grupos de nodos de AKS, no se puede actualizar el perfil del grupo de nodos inicial una vez creado el clúster de AKS. Este comportamiento significa que no se puede actualizar una plantilla de Resource Manager existente, realice un cambio en los grupos de nodos y volver a implementar. En su lugar, debe crear una plantilla de Resource Manager independiente que actualiza los grupos de agentes para un clúster AKS existente.

Crear una plantilla como `aks-agentpools.json` y pegue el siguiente ejemplo de manifiesto. Esta plantilla de ejemplo configura los valores siguientes:

* Las actualizaciones de la *Linux* grupo agente denominado *myagentpool* para ejecutar los tres nodos.
* Establece los nodos en el grupo de nodos para ejecutar la versión de Kubernetes *1.12.8*.
* Define el tamaño del nodo como *Standard_DS2_v2*.

Edite estos valores como necesite para actualizar, agregar o eliminar grupos de nodos según sea necesario:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "clusterName": {
      "type": "string",
      "metadata": {
        "description": "The name of your existing AKS cluster."
      }
    },
    "location": {
      "type": "string",
      "metadata": {
        "description": "The location of your existing AKS cluster."
      }
    },
    "agentPoolName": {
      "type": "string",
      "defaultValue": "myagentpool",
      "metadata": {
        "description": "The name of the agent pool to create or update."
      }
    },
    "vnetSubnetId": {
      "type": "string",
      "defaultValue": "",
      "metadata": {
        "description": "The Vnet subnet resource ID for your existing AKS cluster."
      }
    }
  },
  "variables": {
    "apiVersion": {
      "aks": "2019-04-01"
    },
    "agentPoolProfiles": {
      "maxPods": 30,
      "osDiskSizeGB": 0,
      "agentCount": 3,
      "agentVmSize": "Standard_DS2_v2",
      "osType": "Linux",
      "vnetSubnetId": "[parameters('vnetSubnetId')]"
    }
  },
  "resources": [
    {
      "apiVersion": "2019-04-01",
      "type": "Microsoft.ContainerService/managedClusters/agentPools",
      "name": "[concat(parameters('clusterName'),'/', parameters('agentPoolName'))]",
      "location": "[parameters('location')]",
      "properties": {
            "maxPods": "[variables('agentPoolProfiles').maxPods]",
            "osDiskSizeGB": "[variables('agentPoolProfiles').osDiskSizeGB]",
            "count": "[variables('agentPoolProfiles').agentCount]",
            "vmSize": "[variables('agentPoolProfiles').agentVmSize]",
            "osType": "[variables('agentPoolProfiles').osType]",
            "storageProfile": "ManagedDisks",
      "type": "VirtualMachineScaleSets",
            "vnetSubnetID": "[variables('agentPoolProfiles').vnetSubnetId]",
            "orchestratorVersion": "1.12.8"
      }
    }
  ]
}
```

Implementar esta plantilla mediante el [Crear implementación de un grupo az] [ az-group-deployment-create] de comandos, tal como se muestra en el ejemplo siguiente. Se le solicitará el nombre del clúster de AKS y la ubicación existente:

```azurecli-interactive
az group deployment create \
    --resource-group myResourceGroup \
    --template-file aks-agentpools.json
```

Puede tardar unos minutos en actualizar el clúster de AKS según la configuración del nodo grupo y las operaciones que defina en la plantilla de Resource Manager.

## <a name="clean-up-resources"></a>Limpieza de recursos

En este artículo, ha creado un clúster de AKS que incluya los nodos basados en GPU. Para reducir los costos innecesarios, es posible que desee eliminar el *gpunodepool*, o todo el clúster AKS.

Para eliminar el grupo de nodos basado en GPU, use el [eliminar az aks nodepool] [ az-aks-nodepool-delete] comando tal como se muestra en el ejemplo siguiente:

```azurecli-interactive
az aks nodepool delete -g myResourceGroup --cluster-name myAKSCluster --name gpunodepool
```

Para eliminar el clúster propiamente dicho, utilice el [eliminación del grupo az] [ az-group-delete] comando para eliminar el grupo de recursos AKS:

```azurecli-interactive
az group delete --name myResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, aprendió a crear y administrar varios grupos de nodos en un clúster de AKS. Para obtener más información acerca de cómo controlar los pods a través de grupos de nodos, vea [procedimientos recomendados para las características avanzadas de programador en AKS][operator-best-practices-advanced-scheduler].

Para crear y usar grupos de nodos de contenedor de Windows Server, vea [crear un contenedor de Windows Server en AKS][aks-windows].

<!-- EXTERNAL LINKS -->
[kubernetes-drain]: https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-taint]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#taint
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- INTERNAL LINKS -->
[azure-cli-install]: /cli/azure/install-azure-cli
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-get-credentials]: /cli/azure/aks#az-aks-get-credentials
[az-group-create]: /cli/azure/group#az-group-create
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-aks-nodepool-add]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-nodepool-add
[az-aks-nodepool-list]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-nodepool-list
[az-aks-nodepool-upgrade]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-nodepool-upgrade
[az-aks-nodepool-scale]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-nodepool-scale
[az-aks-nodepool-delete]: /cli/azure/ext/aks-preview/aks#ext-aks-preview-az-aks-nodepool-delete
[vm-sizes]: ../virtual-machines/linux/sizes.md
[taints-tolerations]: operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations
[gpu-cluster]: gpu-cluster.md
[az-group-delete]: /cli/azure/group#az-group-delete
[install-azure-cli]: /cli/azure/install-azure-cli
[supported-versions]: supported-kubernetes-versions.md
[operator-best-practices-advanced-scheduler]: operator-best-practices-advanced-scheduler.md
[aks-windows]: windows-container-cli.md
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
