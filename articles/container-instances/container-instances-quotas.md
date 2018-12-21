---
title: Disponibilidad de regiones y cuotas en Azure Container Instances
description: La disponibilidad de regiones y cuotas predeterminadas del servicio Azure Container Instances.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 12/07/2018
ms.author: danlep
ms.openlocfilehash: a7b61702feb062c57fdec84f335ace44a47d0283
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "53249488"
---
# <a name="quotas-and-region-availability-for-azure-container-instances"></a>Disponibilidad de cuotas y regiones en Azure Container Instances

Todos los servicios de Azure incluyen ciertas cuotas y límites predeterminados para los recursos y las características. En las secciones siguientes se detallan los límites de recursos predeterminados para varios recursos de Azure Container Instances (ACI), así como la disponibilidad del servicio ACI en regiones de Azure.

## <a name="service-quotas-and-limits"></a>Límites y cuotas del servicio

[!INCLUDE [container-instances-limits](../../includes/container-instances-limits.md)]

## <a name="region-availability"></a>Disponibilidad en regiones

Azure Container Instances está disponible en las siguientes regiones con los límites de memoria y de CPU especificados.

| Ubicación | SO | CPU | Memoria (GB) |
| -------- | -- | :---: | :-----------: |
| Este de EE. UU., Europa del Norte, Europa Occidental, Oeste de EE. UU., Oeste de EE. UU. 2 | Linux | 4 | 14 |
| Este de Japón | Linux | 2 | 8 |
| Este de Australia, Este de EE. UU. 2, Sudeste Asiático | Linux | 2 | 7 |
| Centro de Canadá, Centro de la India, Asia Oriental, Centro-norte de EE. UU., Centro-sur de EE. UU. | Linux | 2 | 3,5 |
| Este de EE. UU., Europa Occidental, Oeste de EE. UU. |  Windows | 4 | 14 |
| Este de Australia, Centro de Canadá, Centro de la India, Asia Oriental, Este de EE. UU. 2, Japón Oriental, Centro-norte de EE. UU., Europa del Norte, Centro-sur de EE. UU., Sudeste Asiático, Oeste de EE. UU. 2 |  Windows | 2 | 3,5 |

Las instancias de contenedor creadas dentro de estos límites de recursos están sujetas a disponibilidad dentro de la región de implementación. Cuando una región está sometida a mucha carga, puede experimentar un error al implementar instancias. Para mitigar este tipo de error de implementación, intente implementar las instancias con una configuración inferior de memoria y de CPU, o intente realizar la implementación en un momento posterior.

Indique al equipo si se necesitan más regiones o aumentar los límites de CPU o memoria en [aka.ms/aci/feedback](https://aka.ms/aci/feedback).

Para más información sobre cómo solucionar problemas de la implementación de instancias de contenedor, consulte [Solución de problemas en las implementaciones en Azure Container Instances](container-instances-troubleshooting.md).

## <a name="next-steps"></a>Pasos siguientes

Pueden aumentarse ciertas cuotas y límites predeterminados. Para solicitar un aumento de uno o varios recursos que admiten un aumento de este tipo, envíe una [solicitud de soporte técnico de Azure] [azure-support] (seleccione "Cuota" en **Tipo de emisión**).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
