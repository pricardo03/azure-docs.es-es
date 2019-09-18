---
title: Usar Availability Zones en Azure Kubernetes Service (AKS)
description: Aprenda a crear un clúster que distribuya nodos a través de las zonas de disponibilidad en Azure Kubernetes Service (AKS)
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 06/24/2019
ms.author: mlearned
ms.openlocfilehash: 4d76578de0c80570e67db03046c42985500ddcdb
ms.sourcegitcommit: 083aa7cc8fc958fc75365462aed542f1b5409623
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70914725"
---
# <a name="preview---create-an-azure-kubernetes-service-aks-cluster-that-uses-availability-zones"></a>Versión preliminar: cree un clúster de Azure Kubernetes Service (AKS) que use zonas de disponibilidad

Un clúster de Azure Kubernetes Service (AKS) distribuye recursos como los nodos y el almacenamiento en secciones lógicas de la infraestructura de procesos subyacente de Azure. Este modelo de implementación garantiza que los nodos se ejecuten en dominios de actualización y error separados en un solo centro de datos de Azure. Los clústeres de AKS implementados con este comportamiento predeterminado proporcionan un alto nivel de disponibilidad para protegerle contra un error de hardware o un evento de mantenimiento planeado.

Para proporcionar un mayor nivel de disponibilidad a sus aplicaciones, los clústeres de AKS se pueden distribuir en zonas de disponibilidad. Estas zonas son centros de datos físicamente separados dentro de una región determinada. Cuando los componentes del clúster se distribuyen entre varias zonas, su clúster de AKS puede tolerar un error en una de esas zonas. Sus aplicaciones y operaciones de administración continuarán disponibles incluso si todo un centro de datos tiene problemas.

En este artículo le mostraremos cómo crear un clúster de AKS y distribuir los componentes del nodo en las zonas de disponibilidad. Esta funcionalidad actualmente está en su versión preliminar.

> [!IMPORTANT]
> Las características en vista previa de AKS son de autoservicio y se tienen que habilitar. Las versiones preliminares se proporcionan "tal cual" y "como están disponibles", y están excluidas de los contratos de nivel de servicio y la garantía limitada. Las versiones preliminares de AKS reciben cobertura parcial del soporte al cliente en la medida de lo posible. Por lo tanto, estas características no están diseñadas para usarse en producción. Para obtener información adicional, consulte los siguientes artículos de soporte:
>
> * [Directivas de soporte técnico para AKS][aks-support-policies]
> * [Preguntas más frecuentes de soporte técnico de Azure][aks-faq]

## <a name="before-you-begin"></a>Antes de empezar

Es necesario tener instalada y configurada la versión 2.0.66 de la CLI de Azure o una versión posterior. Ejecute  `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte  [Install Azure CLI][install-azure-cli] (Instalación de la CLI de Azure).

### <a name="install-aks-preview-cli-extension"></a>Instalación de la extensión aks-preview de la CLI

Para crear clústeres de AKS que usen zonas de disponibilidad, necesita la versión 0.4.12 o superior de la extensión de CLI *aks-preview*. Instale la extensión de la CLI de Azure *aks-preview* con el comando [az extension add][az-extension-add] y, a continuación, busque las actualizaciones disponibles con el comando [az extension update][az-extension-update]:

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

### <a name="register-the-availabilityzonepreview-feature-flag-for-your-subscription"></a>Registro de la marca de características de AvailabilityZonePreview para la suscripción

Para crear un clúster de AKS con zonas de disponibilidad, primero debe habilitar la marca de características *AvailabilityZonePreview* en su suscripción. Para registrar la marca de características *AvailabilityZonePreview*, use el comando [az feature register][az-feature-register] tal como se muestra en el siguiente ejemplo:

> [!CAUTION]
> Actualmente, al registrar una característica en una suscripción, no se puede anular el registro de esa característica. Después de habilitar algunas características en vista previa, se pueden usar los valores predeterminados en todos los clústeres de AKS y, luego, se pueden crear en la suscripción. No habilite características en vista previa en las suscripciones de producción. Use una suscripción independiente para probar las características en vista previa y recopilar comentarios.

```azurecli-interactive
az feature register --name AvailabilityZonePreview --namespace Microsoft.ContainerService
```

Tarda unos minutos en que el estado muestre *Registrado*. Puede comprobar el estado de registro con el comando [az feature list][az-feature-list]:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AvailabilityZonePreview')].{Name:name,State:properties.state}"
```

Cuando todo esté listo, actualice el registro del proveedor de recursos *Microsoft.ContainerService* con el comando [az provider register][az-provider-register]:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="limitations-and-region-availability"></a>Limitaciones y disponibilidad de región

Los clústeres de AKS se pueden crear actualmente mediante zonas de disponibilidad en las siguientes regiones:

* Este de EE. UU. 2
* Europa del Norte
* Sudeste asiático
* Europa occidental
* Oeste de EE. UU. 2

Las siguientes limitaciones se aplican cuando crea un clúster de AKS mediante zonas de disponibilidad:

* Solo puede habilitar las zonas de disponibilidad cuando se crea el clúster.
* La configuración de la zona de disponibilidad no se puede actualizar después de crear el clúster. Tampoco puede actualizar un clúster de zona sin disponibilidad para usar zonas de disponibilidad.
* No puede deshabilitar las zonas de disponibilidad de un clúster de AKS una vez que se haya creado.
* El tamaño del nodo (SKU de VM) seleccionado debe estar disponible en todas las zonas de disponibilidad.
* Los clústeres con zonas de disponibilidad habilitadas requieren el uso de equilibradores de carga estándar de Azure para la distribución entre zonas.
* Debe usar la versión 1.13.5 o superior de Kubernetes para implementar los equilibradores de carga estándar.

Los clústeres de AKS que usan zonas de disponibilidad deben usar el SKU *estándar* del equilibrador de carga de Azure. La SKU *básica* predeterminada del equilibrador de carga de Azure no admite la distribución entre zonas de disponibilidad. Para obtener más información y las limitaciones del equilibrador de carga estándar, consulte las [limitaciones de la SKU estándar del equilibrador de carga de Azure][standard-lb-limitations].

### <a name="azure-disks-limitations"></a>Limitaciones de los discos de Azure

Los volúmenes que usan discos administrados de Azure no son recursos de zona actualmente. Los pods reprogramados en una zona diferente a su zona original no pueden volver a conectarse a sus discos anteriores. Es recomendable ejecutar cargas de trabajo sin estado que no requieran un almacenamiento persistente, ya que pueden surgir problemas en las zonas.

Si debe ejecutar cargas de trabajo con estado, use marcas y tolerancias en sus especificaciones de pod para indicarle al programador de Kubernetes que cree pods en la misma zona que sus discos. Asimismo, puede usar el almacenamiento basado en la red, como Azure Files, que se puede conectar a los pods según se programan entre zonas.

## <a name="overview-of-availability-zones-for-aks-clusters"></a>Descripción general de las zonas de disponibilidad para los clústeres de AKS

Las zonas de disponibilidad son una oferta que protege las aplicaciones y datos de los errores del centro de datos. Las zonas son ubicaciones físicas exclusivas dentro de una región de Azure. Cada zona de disponibilidad consta de uno o varios centros de datos equipados con alimentación, refrigeración y redes independientes. Para garantizar la resistencia, hay tres zonas independientes como mínimo en todas las regiones habilitadas. La separación física de las zonas de disponibilidad dentro de una región protege las aplicaciones y los datos frente a los errores del centro de datos. Los servicios con redundancia de zona replican las aplicaciones y los datos entre zonas de disponibilidad para protegerlos frente a puntos de error únicos.

Para obtener más información, consulte [¿Qué son las zonas de disponibilidad en Azure?][az-overview]

Los clústeres de AKS que se implementan mediante zonas de disponibilidad que pueden distribuir nodos en varias zonas dentro de una sola región. Por ejemplo, un clúster en la región  *Este de EE. UU. 2* puede crear nodos en las tres zonas de disponibilidad del *Este de EE. UU. 2*. Esta distribución de los recursos del clúster de AKS mejora la disponibilidad del clúster, ya que son resistentes a los errores de una zona específica.

![Distribución de nodos de AKS en zonas de disponibilidad](media/availability-zones/aks-availability-zones.png)

Cuando se produce una interrupción en una zona, los nodos se pueden reequilibrar manualmente o con el escalador automático del clúster. Así pues, si una sola zona deja de estar disponible, sus aplicaciones continúan ejecutándose.

## <a name="create-an-aks-cluster-across-availability-zones"></a>Crear un clúster de AKS en zonas de disponibilidad

Cuando crea un clúster con el comando [az aks create][az-aks-create], el parámetro `--node-zones` define en qué zonas se implementan los nodos de agente. Los componentes del plano de control de AKS del clúster también se extienden a través de las zonas de la configuración más alta disponible, cuando crea un clúster que especifique el parámetro `--node-zones`.

Si no define ninguna zona para el grupo de agentes predeterminado cuando crea un clúster de AKS, los componentes del plano de control AKS del clúster no usarán las zonas de disponibilidad. Puede agregar grupos de nodos adicionales (actualmente en versión preliminar en AKS) mediante el comando [az aks nodepool add][az-aks-nodepool-add] y especificar `--node-zones` para esos nuevos nodos de agente; sin embargo, los componentes del plano de control permanecen sin conocimiento de la zona de disponibilidad. No puede cambiar el reconocimiento de zona para un grupo de nodos o los componentes del plano de control de AKS una vez que se implementan.

En el ejemplo siguiente se crea un clúster de AKS denominado *myAKSCluster* en el grupo de recursos denominado *myResourceGroup*. Se crean un total de *3* nodos: un agente en la zona *1*, uno en la *2* y otro en la *3*. Los componentes del plano de control de AKS también se distribuyen a través de zonas en la configuración más alta disponible, ya que se definen como parte del proceso de creación del clúster.

```azurecli-interactive
az group create --name myResourceGroup --location eastus2

az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --generate-ssh-keys \
    --vm-set-type VirtualMachineScaleSets \
    --load-balancer-sku standard \
    --node-count 3 \
    --node-zones 1 2 3
```

El clúster de AKS tarda unos minutos en crearse.

## <a name="verify-node-distribution-across-zones"></a>Comprobar la distribución de nodos entre zonas

Cuando el clúster esté listo, enumere los nodos de agente en el conjunto de escalado para ver en qué zona de disponibilidad se implementarán.

Primero, obtenga las credenciales del clúster de AKS mediante el comando [az aks get-credentials][az-aks-get-credentials]:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name myAKSCluster
```

A continuación, use el comando [kubectl describe][kubectl-describe] para enumerar los nodos del clúster. Filtre el valor *failure-domain.beta.kubernetes.io/zone* tal como se muestra en el siguiente ejemplo:

```console
kubectl describe nodes | grep -e "Name:" -e "failure-domain.beta.kubernetes.io/zone"
```

El siguiente resultado de ejemplo muestra los tres nodos distribuidos en la región especificada y las zonas de disponibilidad, como *eastus2-1* para la primera zona de disponibilidad y *eastus2-2* para la segunda zona de disponibilidad:

```console
Name:       aks-nodepool1-28993262-vmss000000
            failure-domain.beta.kubernetes.io/zone=eastus2-1
Name:       aks-nodepool1-28993262-vmss000001
            failure-domain.beta.kubernetes.io/zone=eastus2-2
Name:       aks-nodepool1-28993262-vmss000002
            failure-domain.beta.kubernetes.io/zone=eastus2-3
```

A medida que agrega nodos adicionales a un grupo de agentes, la plataforma Azure distribuye automáticamente las VM subyacentes en las zonas de disponibilidad especificadas.

## <a name="next-steps"></a>Pasos siguientes

En este artículo se detalla cómo crear un clúster de AKS que usa zonas de disponibilidad. Para obtener más información sobre los clústeres de alta disponibilidad, consulte [Best practices for business continuity and disaster recovery in AKS][best-practices-bc-dr] (Procedimientos recomendados para la continuidad empresarial y recuperación ante desastres en AKS).

<!-- LINKS - internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[az-aks-create]: /cli/azure/aks#az-aks-create
[az-overview]: ../availability-zones/az-overview.md
[best-practices-bc-dr]: operator-best-practices-multi-region.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[standard-lb-limitations]: load-balancer-standard.md#limitations
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[az-aks-nodepool-add]: /cli/azure/ext/aks-preview/aks/nodepool#ext-aks-preview-az-aks-nodepool-add
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials

<!-- LINKS - external -->
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
