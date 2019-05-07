---
title: Compatible con los recursos de Azure Red Hat OpenShift | Microsoft Docs
description: Comprender qué regiones de Azure y tamaños de máquina virtual son compatibles con Microsoft Azure Red Hat OpenShift.
services: container-service
author: tylermsft
ms.author: twhitney
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/06/2019
ms.openlocfilehash: 22cc195b7808ad4a9111aafcf883a68c51f3709f
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65076111"
---
# <a name="azure-red-hat-openshift-resources"></a>Recursos de Azure Red Hat OpenShift

En este tema se enumera las regiones de Azure y tamaños de máquina virtual compatibles con el servicio de Microsoft Azure Red Hat OpenShift.

## <a name="azure-regions"></a>Regiones de Azure

Puede implementar clústeres de Azure Red Hat OpenShift en las siguientes regiones de Azure en todo el mundo:

|Region|Código de la CLI|
|-|-|
|Este de Australia 🇦🇺|`australiaeast`|
|🇨🇦 Canada Central|`canadacentral`|
|🇨🇦 Canada East|`canadaeast`|
|🇺🇸 East US|`eastus`|
|🇺🇸 Oeste de Estados Unidos|`westus`|
|Europa occidental 🇪🇺|`westeurope`|
|Europa del Norte 🇪🇺|`northeurope`|

## <a name="virtual-machine-sizes"></a>Tamaños de máquina virtual

Estos son los tamaños de máquina virtual admitidas que puede especificar para los nodos de proceso en el clúster de Azure Red Hat OpenShift.

> [!Important]
> Cada máquina virtual tiene un número diferente de las unidades que se pueden conectar. Esto puede no ser tan inmediatamente claro como el tamaño de la CPU o memoria.
> No todos los tamaños de máquina virtual están disponibles en todas las regiones. Aunque la API es compatible con el tamaño que especifique, podría obtener un error si el tamaño no está disponible en la región que especifique.
> Consulte [tamaños de la lista actual de la máquina virtual admitido por región](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines) para obtener más información.

## <a name="compute-node-sizes"></a>Los tamaños de nodo de proceso

Los siguientes tamaños de nodo de proceso son compatibles con la API de REST de Azure Red Hat OpenShift:

|Tamaño|vCPU|RAM|
|-|-|-|
|Standard D4s v3|4|16 GB|
|Standard D8s v3|8|32 GB|
|Standard D16s v3|16|64 GB|
|Standard D32s v3|32|128 GB|
|-|-|-|
|Standard E4s v3|4|32 GB|
|Standard E8s v3|8|64 GB|
|Standard E16s v3|16|128 GB|
|Standard E32s v3|32|256 GB|
|-|-|-|
|F8s v2 estándar|8|16 GB|
|F16s v2 estándar|16|32 GB|
|F32s v2 estándar|32|64 GB|

## <a name="master-node-sizes"></a>Tamaños de nodo principal

El siguiente patrón / tamaños de nodo de infraestructura son compatibles con la API de REST de Azure Red Hat OpenShift:

|Tamaño|vCPU|RAM|
|-|-|-|
|Standard D4s v3|4|16 GB|
|Standard D8s v3|8|32 GB|
|Standard D16s v3|16|64 GB|
|Standard D32s v3|32|128 GB|

## <a name="next-steps"></a>Pasos siguientes

Pruebe el [crear un clúster de Azure Red Hat OpenShift](tutorial-create-cluster.md) tutorial.