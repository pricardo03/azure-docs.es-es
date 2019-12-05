---
title: 'Creación de un clúster de Kubernetes con Azure Dev Spaces habilitado: Azure Cloud Shell'
services: azure-dev-spaces
ms.date: 10/04/2018
ms.topic: conceptual
description: Aprenda a crear rápidamente un clúster de Kubernetes habilitado para Azure Dev Spaces directamente desde el explorador sin instalar nada.
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, contenedores, Helm, service mesh, enrutamiento de service mesh, kubectl, k8s
ms.openlocfilehash: ac825caa7aacbc6ac21dd1f9fe7acbb9cf12e900
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/22/2019
ms.locfileid: "74325768"
---
# <a name="create-a-kubernetes-cluster-with-azure-dev-spaces-enabled-with-azure-cloud-shell"></a>Creación de un clúster de Kubernetes con Azure Dev Spaces habilitado con Azure Cloud Shell

Puede usar [Azure Cloud Shell](/azure/cloud-shell) para crear un clúster de Azure Kubernetes Service mediante el botón **Probar** de esta página. Si no ha iniciado sesión, siga los avisos para iniciar sesión con una cuenta de Azure y, luego, escriba los comandos en el símbolo del sistema de Azure Cloud Shell cuando aparezca.

## <a name="create-the-cluster"></a>Creación de clústeres

En primer lugar, cree el grupo de recursos en una [región que admita Azure Dev Spaces][supported-regions].

```azurecli-interactive
az group create --name MyResourceGroup --location <region>
```

Crear un clúster de Kubernetes con el siguiente comando:

```azurecli-interactive
az aks create -g MyResourceGroup -n MyAKS --location <region> --disable-rbac --generate-ssh-keys
```

La operación de creación del clúster tarda unos minutos.  Cuando haya finalizado, la salida se muestra en el formato JSON. Busque `provisioningState` y compruebe que su valor es `Succeeded`.

## <a name="next-steps"></a>Pasos siguientes

Consulte [Azure Dev Spaces](/azure/dev-spaces/) para obtener vínculos a tutoriales completos.

> [!IMPORTANT]
> Muchas de los tutoriales e inicios rápidos de Azure Dev Spaces usan la CLI de Azure Dev Spaces para realizar operaciones. La CLI de Azure Dev Spaces no se puede instalar en Azure Cloud Shell.


[supported-regions]: ../about.md#supported-regions-and-configurations