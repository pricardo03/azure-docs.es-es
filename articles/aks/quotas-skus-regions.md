---
title: Las cuotas, SKU y la disponibilidad de regiones en Azure Kubernetes Service (AKS)
description: Obtenga información sobre las cuotas predeterminadas, tamaños de SKU de máquina virtual de nodo restringido y disponibilidad en regiones de Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
ms.service: container-service
ms.topic: conceptual
ms.date: 04/09/2019
ms.author: iainfou
ms.openlocfilehash: 8d4ed8f791858747814972bcf16a9672a7f12610
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65901455"
---
# <a name="quotas-virtual-machine-size-restrictions-and-region-availability-in-azure-kubernetes-service-aks"></a>Cuotas, las restricciones de tamaño de máquina virtual y la disponibilidad de regiones en Azure Kubernetes Service (AKS)

Todos los servicios de Azure establece límites predeterminados y cuotas de recursos y características. Ciertas SKU de máquina virtual (VM) también está restringidos para su uso.

En este artículo se detalla los límites de recursos predeterminado para los recursos de Azure Kubernetes Service (AKS) y la disponibilidad de AKS en las regiones de Azure.

## <a name="service-quotas-and-limits"></a>Límites y cuotas del servicio

[!INCLUDE [container-service-limits](../../includes/container-service-limits.md)]

## <a name="provisioned-infrastructure"></a>Infraestructura aprovisionada

A la infraestructura aprovisionada se le aplican las demás limitaciones de red, proceso y almacenamiento. Para conocer los límites pertinentes, consulte [suscripción de Azure y límites de servicio](../azure-subscription-service-limits.md).

> [!IMPORTANT]
> Al actualizar un clúster de AKS, temporalmente se usan recursos adicionales. Estos recursos incluyen direcciones IP disponibles en una subred de red virtual o la cuota de vCPU de máquina virtual. Si usa contenedores de Windows Server (actualmente en versión preliminar de AKS), es el único enfoque aprobado para aplicar las actualizaciones más recientes para los nodos realizar una operación de actualización. Un proceso de actualización de clúster con error puede indicar que no tiene la IP direcciones espacio o vCPU cuota disponible para controlar estos recursos temporales. Para obtener más información sobre el proceso de actualización del nodo de Windows Server, vea [actualizar un grupo de nodos de AKS][nodepool-upgrade].

## <a name="restricted-vm-sizes"></a>Tamaños de máquina virtual restringidos

Cada nodo en un clúster de AKS contiene una cantidad fija de recursos de proceso, como la memoria y vCPU. Si un nodo de AKS contiene recursos de proceso suficiente, pods podrían no ejecutarse correctamente. Para asegurarse de que el necesario *kube-system* pods y las aplicaciones de forma confiable puede programarse, no use las siguientes SKU de máquina virtual en AKS:

- Standard_A0
- Standard_A1
- Standard_A1_v2
- Standard_B1s
- Standard_B1ms
- Standard_F1
- Standard_F1s

Para obtener más información sobre los tipos de máquinas virtuales y sus recursos de proceso, consulte [tamaños de máquinas virtuales en Azure][vm-skus].

## <a name="region-availability"></a>Disponibilidad por región

Para obtener la lista más reciente del lugar donde pueda implementar y ejecutar clústeres, consulte [disponibilidad por región AKS][region-availability].

## <a name="next-steps"></a>Pasos siguientes

Pueden aumentarse ciertas cuotas y límites predeterminados. Si el recurso es compatible con un aumento, solicitar el aumento a través de un [solicitud de soporte técnico de Azure] [ azure-support] (para **tipo de problema**, seleccione **cuota** ).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
[region-availability]: https://azure.microsoft.com/global-infrastructure/services/?products=kubernetes-service

<!-- LINKS - Internal -->
[vm-skus]: ../virtual-machines/linux/sizes.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
