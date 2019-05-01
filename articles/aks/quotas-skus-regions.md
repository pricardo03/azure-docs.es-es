---
title: Las cuotas, SKU y la disponibilidad de regiones en Azure Kubernetes Service (AKS)
description: Obtenga información sobre las cuotas predeterminadas, tamaños de SKU de máquina virtual de nodo restringido y disponibilidad en regiones de Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 04/09/2019
ms.author: iainfou
ms.openlocfilehash: abeb9ef6e467b62cf7332e01e1b77c710b9ba4f4
ms.sourcegitcommit: 524625dd12e0537173616a991802075e2dc9da12
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "64413026"
---
# <a name="quotas-virtual-machine-size-restrictions-and-region-availability-in-azure-kubernetes-service-aks"></a>Cuotas, las restricciones de tamaño de máquina virtual y la disponibilidad de regiones en Azure Kubernetes Service (AKS)

Todos los servicios de Azure incluyen ciertas cuotas y límites predeterminados para los recursos y las características. Tamaño del nodo de determinadas máquinas virtuales (VM) las SKU son, a continuación, restringida para su uso.

En este artículo se detalla los límites de recursos predeterminado para los recursos de Azure Kubernetes Service (AKS), así como la disponibilidad del servicio AKS en las regiones de Azure.

## <a name="service-quotas-and-limits"></a>Límites y cuotas del servicio

[!INCLUDE [container-service-limits](../../includes/container-service-limits.md)]

## <a name="provisioned-infrastructure"></a>Infraestructura aprovisionada

A la infraestructura aprovisionada se le aplican las demás limitaciones de red, proceso y almacenamiento. Consulte el artículo sobre los [límites de suscripción y servicios de Azure](../azure-subscription-service-limits.md) para conocer los límites pertinentes.

## <a name="restricted-vm-sizes"></a>Tamaños de máquina virtual restringidos

Cada nodo en un clúster de AKS contiene una cantidad fija de recursos de proceso, como la memoria y vCPU. Si un nodo de AKS contiene recursos de proceso suficiente, se pueden producir un error en los pods para ejecutarse correctamente. Para asegurarse de que el necesario *kube-system* confiable se pueden programar los pods y sus aplicaciones, no se puede usar las siguientes SKU de máquina virtual en AKS:

- Standard_A0
- Standard_A1
- Standard_A1_v2
- Standard_B1s
- Standard_B1ms
- Standard_F1
- Standard_F1s

Para obtener más información sobre los tipos de máquinas virtuales y sus recursos de proceso, consulte [tamaños de máquinas virtuales en Azure][vm-skus].

## <a name="region-availability"></a>Disponibilidad en regiones

Para obtener la lista más reciente del lugar donde pueda implementar y ejecutar clústeres, consulte [disponibilidad por región AKS][region-availability].

## <a name="next-steps"></a>Pasos siguientes

Pueden aumentarse ciertas cuotas y límites predeterminados. Para solicitar un aumento de uno o varios recursos que admiten un aumento de este tipo, envíe una [solicitud de soporte técnico de Azure] [azure-support] (seleccione "Cuota" en **Tipo de emisión**).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
[region-availability]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service

<!-- LINKS - Internal -->
[vm-skus]: ../virtual-machines/linux/sizes.md
