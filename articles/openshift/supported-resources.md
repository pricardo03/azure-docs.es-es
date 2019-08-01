---
title: Recursos admitidos para Red Hat OpenShift en Azure | Microsoft Docs
description: Descripción de las regiones de Azure y los tamaños de máquina virtual admitidos por Red Hat OpenShift en Microsoft Azure.
services: container-service
author: jimzim
ms.author: jzim
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 05/15/2019
ms.openlocfilehash: c226227797802ab58d1bcbaadb7e97e780b30560
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66306213"
---
# <a name="azure-red-hat-openshift-resources"></a>Recursos de Red Hat OpenShift en Azure

En este tema se enumeran las regiones de Azure y los tamaños de máquina virtual admitidos por el servicio Red Hat OpenShift en Microsoft Azure.

## <a name="azure-regions"></a>Regiones de Azure

Consulte [Productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=openshift&regions=all) para una lista actualizada de regiones en las que puede implementar clústeres de Red Hat OpenShift en Azure.

## <a name="virtual-machine-sizes"></a>Tamaños de máquina virtual

Estos son los tamaños de máquina virtual admitidos que puede especificar para los nodos de ejecución del clúster de Red Hat OpenShift en Azure.

> [!Important]
> Cada máquina virtual tiene un número diferente de unidades que se pueden conectar. Esto puede no ser tan inmediatamente claro como el tamaño de la CPU o la memoria.
> No todos los tamaños de máquina virtual están disponibles en todas las regiones. Aunque la API admita el tamaño especificado, podría obtener un error si el tamaño no está disponible en la región que especifique.
> Consulte [Lista actual de tamaños de máquina virtual admitidos por región](https://azure.microsoft.com/global-infrastructure/services/?products=virtual-machines) para más información.

## <a name="compute-node-sizes"></a>Tamaños de nodo de ejecución

Los siguientes tamaños de nodo de ejecución son compatibles con la API REST de Red Hat OpenShift en Azure:

|Tamaño|vCPU|RAM|
|-|-|-|
|D4s v3 Estándar|4|16 GB|
|D8s v3 Estándar|8|32 GB|
|D16s v3 Estándar|16|64 GB|
|D32s v3 Estándar|32|128 GB|
|-|-|-|
|E4s v3 Estándar|4|32 GB|
|E8s v3 Estándar|8|64 GB|
|E16s v3 Estándar|16|128 GB|
|E32s v3 Estándar|32|256 GB|
|-|-|-|
|F8s v2 Estándar|8|16 GB|
|F16s v2 Estándar|16|32 GB|
|F32s v2 Estándar|32|64 GB|

## <a name="master-node-sizes"></a>Tamaños del nodo maestro

Los siguientes tamaños de nodo maestro o de infraestructura son compatibles con la API REST de Red Hat OpenShift en Azure:

|Tamaño|vCPU|RAM|
|-|-|-|
|D4s v3 Estándar|4|16 GB|
|D8s v3 Estándar|8|32 GB|
|D16s v3 Estándar|16|64 GB|
|D32s v3 Estándar|32|128 GB|

## <a name="next-steps"></a>Pasos siguientes

Consulte el tutorial [Creación de un clúster de Red Hat OpenShift en Azure](tutorial-create-cluster.md).