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
ms.openlocfilehash: 10ec07cab94184810e083c643e862cebe8e2431c
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2020
ms.locfileid: "77121438"
---
# <a name="preview---use-managed-identities-in-azure-kubernetes-service"></a>Uso de identidades administradas en Azure Kubernetes Service (versión preliminar)

Actualmente, un clúster de Azure Kubernetes Service (AKS) (específicamente, el proveedor de servicios en la nube de Kubernetes) requiere una *entidad de servicio* para crear recursos adicionales, como equilibradores de carga y discos administrados, en Azure. El usuario puede proporcionar una entidad de servicio o bien AKS se encargará de crearla. Las entidades de servicio suelen tener una fecha de expiración. Llega un momento en que los clústeres alcanzan un estado en el que se debe renovar la entidad de servicio para mantener el clúster en funcionamiento. La administración de las entidades de servicio es compleja.

Las *identidades administradas* son básicamente un contenedor relacionado con las entidades de servicio y facilitan su administración. Para más información, consulte el tema sobre [identidades administradas para recursos de Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview).

AKS crea dos identidades administradas:

- **Identidad administrada asignada por el sistema**: identidad que utiliza el proveedor de servicios en la nube de Kubernetes para crear recursos de Azure en nombre del usuario. El ciclo de vida de la identidad asignada por el sistema está vinculado al del clúster. La identidad se elimina cuando se elimina el clúster.
- **Identidad administrada asignada por el usuario**: la identidad que se usa para la autorización en el clúster. Por ejemplo, la identidad asignada por el usuario se usa para autorizar a AKS a usar los registros de control de acceso (ACR) o para autorizar al kubelet a obtener metadatos de Azure.

En este período de versión preliminar, aún será necesario contar con una entidad de servicio. Esta entidad se usa para autorizar complementos como la supervisión, los nodos virtuales, Azure Policy y el enrutamiento de aplicaciones HTTP. Se está trabajando para eliminar la dependencia que tienen los complementos del nombre de la entidad de seguridad de servicio (SPN). Al final desaparecerá el requisito de un SPN en AKS.

> [!IMPORTANT]
> Las características en versión preliminar de AKS están disponibles como opción de participación y autoservicio. Las versiones preliminares se proporcionan "tal cual" y "como están disponibles", y están excluidas de los Acuerdos de Nivel de Servicio y la garantía limitada. Las versiones preliminares de AKS reciben cobertura parcial del soporte al cliente en la medida de lo posible. Por lo tanto, estas características no están diseñadas para usarse en producción. Para más información, consulte los siguientes artículos de soporte:
>
> - [Directivas de soporte técnico para AKS](support-policies.md)
> - [Preguntas más frecuentes de soporte técnico de Azure](faq.md)

## <a name="before-you-begin"></a>Antes de empezar

Debe tener instalados los siguientes recursos:

- CLI de Azure, versión 2.0.70 o posterior
- Extensión aks-preview 0.4.14

Para instalar la extensión aks-preview 0.4.14 o posterior, use los siguientes comandos de la CLI de Azure:

```azurecli
az extension add --name aks-preview
az extension list
```

> [!CAUTION]
> Actualmente, después de registrar una característica en una suscripción no se puede anular el registro de esa característica. Al habilitar algunas características en versión preliminar, es posible que se usen valores predeterminados en todos los clústeres de AKS creados más tarde en la suscripción. No habilite características en vista previa en las suscripciones de producción. En su lugar, use una suscripción independiente para probar las características en versión preliminar y recopilar comentarios.

```azurecli-interactive
az feature register --name MSIPreview --namespace Microsoft.ContainerService
```

Pueden pasar unos minutos hasta que el estado aparezca como **Registrado**. Puede comprobar el estado del registro con el comando [az feature list](https://docs.microsoft.com/cli/azure/feature?view=azure-cli-latest#az-feature-list):

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/MSIPreview')].{Name:name,State:properties.state}"
```

Cuando el estado se muestre como Registrado, actualice el registro del proveedor de recursos `Microsoft.ContainerService` mediante el comando [az provider register](https://docs.microsoft.com/cli/azure/provider?view=azure-cli-latest#az-provider-register):

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="create-an-aks-cluster-with-managed-identities"></a>Creación de un clúster de AKS con identidades administradas

Ahora puede crear un clúster de AKS con identidades administradas utilizando los siguientes comandos de la CLI.

En primer lugar, cree un grupo de recursos de Azure:

```azurecli-interactive
# Create an Azure resource group
az group create --name myResourceGroup --location westus2
```

Después, cree un clúster de AKS:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyManagedCluster --enable-managed-identity
```

Por último, obtenga credenciales para acceder al clúster:

```azurecli-interactive
az aks get-credentials --resource-group myResourceGroup --name MyManagedCluster
```

El clúster se creará en pocos minutos. Después, puede implementar las cargas de trabajo de la aplicación en el nuevo clúster e interactuar con él del mismo modo que con los clústeres de AKS basados en una entidad de servicio.

> [!IMPORTANT]
>
> - Los clústeres de AKS con identidades administradas solo se pueden habilitar durante la creación del clúster.
> - Los clústeres de AKS existentes no se pueden actualizar para habilitar las identidades administradas.
