---
title: Disponibilidad de regiones y cuotas en Azure Container Instances
description: Cuotas, límites y disponibilidad de regiones del servicio Azure Container Instances.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 02/15/2019
ms.author: danlep
ms.openlocfilehash: c676989b4b882f2b1887a1b6a5091b60027f61d0
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2019
ms.locfileid: "56328419"
---
# <a name="quotas-and-region-availability-for-azure-container-instances"></a>Disponibilidad de cuotas y regiones en Azure Container Instances

Todos los servicios de Azure incluyen ciertas cuotas y límites predeterminados para los recursos y las características. En las siguientes secciones se detallan los límites de recursos predeterminados para varios recursos de Azure Container Instances, así como la disponibilidad del servicio en regiones de Azure.

## <a name="service-quotas-and-limits"></a>Límites y cuotas del servicio

[!INCLUDE [container-instances-limits](../../includes/container-instances-limits.md)]

## <a name="feature-availability"></a>Disponibilidad de características

Azure Container Instances puede programar los contenedores Windows y Linux con la misma API. Sin embargo, las siguientes características están actualmente disponibles solo en los grupos de contenedores de Linux. Está prevista la compatibilidad con Windows.

* Varios contenedores por grupo de contenedor
* Montaje del volumen (Azure Files, emptyDir, GitRepo, secreto)
* Red virtual (versión preliminar)
* Recursos de GPU (versión preliminar)

## <a name="region-availability"></a>Disponibilidad en regiones

Azure Container Instances está disponible en las siguientes regiones con los límites de memoria y de CPU especificados para cada grupo de contenedores. Los valores están actualizados en el momento de la publicación. Para obtener información actualizada, use la API de la [lista funcionalidades](/rest/api/container-instances/listcapabilities/listcapabilities). 

La disponibilidad y los límites de recursos pueden diferir cuando se usa Azure Container Instances con una [red virtual](container-instances-vnet.md) (versión preliminar) o con [recursos de GPU](container-instances-gpu.md) (versión preliminar).

| Ubicación | SO | CPU | Memoria (GB) |
| -------- | -- | :---: | :-----------: |
| Centro de Canadá, Centro de EE. UU., Este de EE. UU. 2, Centro-sur de EE. UU. | Linux | 4 | 16 |
| Este de EE. UU., Europa del Norte, Europa Occidental, Oeste de EE. UU., Oeste de EE. UU. 2 | Linux | 4 | 14 |
| Este de Japón | Linux | 2 | 8 |
| Este de Australia, Sudeste Asiático | Linux | 2 | 7 |
| Centro de la India, Asia Oriental, Centro-norte de EE. UU., India meridional | Linux | 2 | 3,5 |
| Este de EE. UU., Europa Occidental, Oeste de EE. UU. |  Windows | 4 | 14 |
| Este de Australia, Centro de Canadá, Centro de la India, Centro de EE. UU., Asia Oriental, Este de EE. UU. 2, Japón Oriental, Centro-norte de EE. UU., Europa del Norte, Centro-sur de EE. UU., India meridional, Sudeste Asiático, Oeste de EE. UU. 2 |  Windows | 2 | 3,5 |

Las instancias de contenedor creadas dentro de estos límites de recursos están sujetas a disponibilidad dentro de la región de implementación. Cuando una región está sometida a mucha carga, puede experimentar un error al implementar instancias. Para mitigar este tipo de error de implementación, intente implementar las instancias con una configuración inferior de memoria y de CPU, o intente realizar la implementación en un momento posterior.

Indique al equipo si se necesitan más regiones o aumentar los límites de CPU o memoria en [aka.ms/aci/feedback](https://aka.ms/aci/feedback).

Para más información sobre cómo solucionar problemas de la implementación de instancias de contenedor, consulte [Solución de problemas en las implementaciones en Azure Container Instances](container-instances-troubleshooting.md).

## <a name="next-steps"></a>Pasos siguientes

Pueden aumentarse ciertas cuotas y límites predeterminados. Para solicitar un aumento de uno o varios recursos que admiten un aumento de este tipo, envíe una [solicitud de soporte técnico de Azure] [azure-support] (seleccione "Cuota" en **Tipo de emisión**).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
