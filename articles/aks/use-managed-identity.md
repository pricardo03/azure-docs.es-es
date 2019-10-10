---
title: Uso de identidades administradas en Azure Kubernetes Service
description: Aprenda a utilizar identidades administradas en Azure Kubernetes Service (AKS)
services: container-service
author: saudas
manager: saudas
ms.service: container-service
ms.topic: article
ms.date: 09/11/2019
ms.author: saudas
ms.openlocfilehash: a5717d8ee44e4d2e086a6e7bc1b7c3d0deb614c8
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827542"
---
# <a name="preview---use-managed-identities-in-azure-kubernetes-service"></a>Uso de identidades administradas en Azure Kubernetes Service (versión preliminar)

En la actualidad, los usuarios deben proporcionar una entidad de servicio o AKS debe crear una en su nombre para que el clúster de AKS (especialmente el proveedor de servicios en la nube de Kubernetes) pueda crear recursos adicionales en Azure, como equilibradores de carga y discos administrados. Las entidades de servicio normalmente se crean con una fecha de expiración. Los clústeres llegarán finalmente a un estado en el que la entidad de servicio deberá renovarse; de lo contrario, el clúster no funcionará. La administración de las entidades de servicio es compleja. En esencia, las identidades administradas son un contenedor relacionado con las entidades de servicio que facilitan su administración. Para más información, consulte este artículo sobre las [entidades administradas](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

AKS crea dos identidades administradas: una asignada por el sistema y otra asignada por el usuario. El proveedor de nube de Kubernetes utiliza una identidad administrada asignada por el sistema para crear recursos de Azure en nombre del usuario. El ciclo de vida de esta identidad administrada depende del clúster y termina cuando se elimina el clúster. AKS también crea una identidad administrada asignada por el usuario que se utiliza en el clúster a fin de autorizar a AKS para que pueda acceder a ACR, de autorizar a kubelet para que pueda obtener metadatos de Azure, etc.

En este período de versión preliminar, aún será necesario contar con una entidad de servicio. Esta entidad de servicio se usará para autorizar ciertos complementos, como la supervisión, el nodo virtual, la directiva de Azure y el enrutamiento de aplicaciones http. Estamos trabajando para poder prescindir de esta dependencia de los complementos del SPN y en algún momento este requisito se eliminará de AKS por completo.

> [!IMPORTANT]
> Las características en vista previa de AKS son de autoservicio y se tienen que habilitar. Las versiones preliminares se proporcionan "tal cual" y "como están disponibles", y están excluidas de los contratos de nivel de servicio y la garantía limitada. Las versiones preliminares de AKS reciben cobertura parcial del soporte al cliente en la medida de lo posible. Por lo tanto, estas características no están diseñadas para usarse en producción. Para obtener información adicional, consulte los siguientes artículos de soporte:
>
> * [Directivas de soporte técnico para AKS](support-policies.md)
> * [Preguntas más frecuentes de soporte técnico de Azure](faq.md)

## <a name="before-you-begin"></a>Antes de empezar

Debe tener lo siguiente:

* También necesita la versión 2.0.70 de la CLI de Azure u otra versión posterior y la extensión 0.4.14 de la versión preliminar de AKS.

## <a name="install-latest-aks-cli-preview-extension"></a>Instalación de la extensión de la versión preliminar de la CLI de AKS más reciente

Necesita la extensión **0.4.14 de la versión preliminar de AKS** o una versión posterior.

```azurecli
az extension update --name aks-preview 
az extension list
```

> [!CAUTION]
> Actualmente, al registrar una característica en una suscripción, no se puede anular el registro de esa característica. Después de habilitar algunas características en vista previa, se pueden usar los valores predeterminados en todos los clústeres de AKS y, luego, se pueden crear en la suscripción. No habilite características en vista previa en las suscripciones de producción. Use una suscripción independiente para probar las características en vista previa y recopilar comentarios.

```azurecli-interactive
az feature register --name MSIPreview --namespace Microsoft.ContainerService
```

Pueden pasar unos minutos hasta que el estado aparezca como *Registrado*. Puede consultar el estado del registro con el comando [az feature list][az-feature-list]:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/MSIPreview')].{Name:name,State:properties.state}"
```

Cuando el estado esté registrado, actualice el registro del proveedor de recursos *Microsoft.ContainerService* utilizando el comando [az provider register][az-provider-register]:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="create-an-aks-cluster-with-managed-identity"></a>Creación de un clúster de AKS con una identidad administrada

Ahora puede crear un clúster de AKS con identidades administradas utilizando el siguiente comando de la CLI.
```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location westus2
```

## <a name="create-an-aks-cluster"></a>Creación de un clúster de AKS
```azurecli-interactive
az aks create -g MyResourceGroup -n MyManagedCluster --enable-managed-identity
```

## <a name="get-credentials-to-access-the-cluster"></a>Obtención de credenciales para acceder al clúster
```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster
```
Después de unos minutos, se crea el clúster. En ese momento, puede implementar las cargas de trabajo de la aplicación e interactuar con ellas igual que con los clústeres de AKS basados en una entidad de servicio. 

> [!IMPORTANT]
> * Los clústeres de AKS con identidades administradas solo se pueden habilitar durante la creación del clúster.
> * Los clústeres de AKS existentes no se pueden actualizar para habilitar las identidades administradas.