---
title: Compatible con los recursos de Azure Red Hat OpenShift | Microsoft Docs
description: Comprender qué regiones de Azure y tamaños de máquina virtual son compatibles con Microsoft Azure Red Hat OpenShift.
services: container-service
author: jimzim
ms.author: jzim
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/15/2019
ms.openlocfilehash: c226227797802ab58d1bcbaadb7e97e780b30560
ms.sourcegitcommit: 009334a842d08b1c83ee183b5830092e067f4374
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66306213"
---
# <a name="azure-red-hat-openshift-resources"></a>Recursos de Azure Red Hat OpenShift

En este tema se enumera las regiones de Azure y tamaños de máquina virtual compatibles con el servicio de Microsoft Azure Red Hat OpenShift.

## <a name="azure-regions"></a>Regiones de Azure

Consulte [productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=openshift&regions=all) clústeres para una lista actualizada de regiones donde puede implementar Azure Red Hat OpenShift.

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
|V3 D4s estándar|4|16 GB|
|V3 D8s estándar|8|32 GB|
|V3 D16s estándar|16|64 GB|
|V3 D32s estándar|32|128 GB|
|-|-|-|
|V3 E4s estándar|4|32 GB|
|V3 E8s estándar|8|64 GB|
|V3 E16s estándar|16|128 GB|
|Standard E32s v3|32|256 GB|
|-|-|-|
|Estándar F8s v2|8|16 GB|
|Estándar F16s v2|16|32 GB|
|Estándar F32s v2|32|64 GB|

## <a name="master-node-sizes"></a>Tamaños de nodo principal

El siguiente patrón / tamaños de nodo de infraestructura son compatibles con la API de REST de Azure Red Hat OpenShift:

|Tamaño|vCPU|RAM|
|-|-|-|
|V3 D4s estándar|4|16 GB|
|V3 D8s estándar|8|32 GB|
|V3 D16s estándar|16|64 GB|
|V3 D32s estándar|32|128 GB|

## <a name="next-steps"></a>Pasos siguientes

Pruebe el [crear un clúster de Azure Red Hat OpenShift](tutorial-create-cluster.md) tutorial.