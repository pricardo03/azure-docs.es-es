---
title: Disponibilidad de regiones y cuotas en Azure Kubernetes Service (AKS)
description: Disponibilidad predeterminada de regiones y cuotas en Azure Kubernetes Service (AKS).
services: container-service
author: iainfoulds
manager: jeconnoc
ms.service: container-service
ms.topic: overview
ms.date: 06/13/2018
ms.author: iainfou
ms.openlocfilehash: 1610ea93eed03fe6efe28e63a7151409e1946f5b
ms.sourcegitcommit: df50934d52b0b227d7d796e2522f1fd7c6393478
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2018
ms.locfileid: "38988721"
---
# <a name="quotas-and-region-availability-for-azure-kubernetes-service-aks"></a>Disponibilidad de regiones y cuotas para Azure Kubernetes Service (AKS)

Todos los servicios de Azure incluyen ciertas cuotas y límites predeterminados para los recursos y las características. En las secciones siguientes se detallan los límites de recursos predeterminados para varios recursos de Azure Kubernetes Service (AKS), así como la disponibilidad del servicio AKS en las regiones de Azure.

## <a name="service-quotas-and-limits"></a>Límites y cuotas del servicio

[!INCLUDE [container-service-limits](../../includes/container-service-limits.md)]

## <a name="provisioned-infrastructure"></a>Infraestructura aprovisionada

A la infraestructura aprovisionada se le aplican las demás limitaciones de red, proceso y almacenamiento. Consulte el artículo sobre los [límites de suscripción y servicios de Azure](../azure-subscription-service-limits.md) para conocer los límites pertinentes.

## <a name="region-availability"></a>Disponibilidad en regiones

Azure Kubernetes Service (AKS) esta disponible en las siguientes regiones:

- Este de Australia
- Centro de Canadá
- Este de Canadá
- Centro de EE. UU.
- Este de EE. UU
- Este de EE. UU. 2
- Este de Japón
- Europa del Norte
- Sur de Reino Unido 2
- Europa occidental
- Oeste de EE. UU.
- Oeste de EE. UU. 2

## <a name="next-steps"></a>Pasos siguientes

Pueden aumentarse ciertas cuotas y límites predeterminados. Para solicitar un aumento de uno o varios recursos que admiten un aumento de este tipo, envíe una [solicitud de soporte técnico de Azure] [azure-support] (seleccione "Cuota" en **Tipo de emisión**).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
