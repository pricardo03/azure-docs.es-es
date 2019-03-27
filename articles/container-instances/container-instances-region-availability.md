---
title: Disponibilidad de recursos para Azure Container Instances
description: Disponibilidad de recursos de proceso y memoria para el servicio Azure Container Instances en diferentes regiones de Azure.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: overview
ms.date: 03/01/2019
ms.author: danlep
ms.openlocfilehash: 1ca23a95c746139963aa70ed20bb888152fd5cd8
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554901"
---
# <a name="resource-availability-for-azure-container-instances-in-azure-regions"></a>Disponibilidad de recursos para Azure Container Instances en las regiones de Azure

En este artículo se detalla la disponibilidad de los recursos de proceso y memoria de Azure Container Instances en las regiones de Azure. 

Los valores que se presentan son los recursos máximos disponibles por cada implementación de un [grupo de contenedores](container-instances-container-groups.md). Los valores están actualizados en el momento de la publicación. Para obtener información actualizada, use la API de la [lista funcionalidades](/rest/api/container-instances/listcapabilities/listcapabilities). 

> [!NOTE]
> Los grupos de contenedores creados dentro de estos límites de recursos están sujetos a disponibilidad en la región de implementación. Cuando una región está sometida a mucha carga, puede experimentar un error al implementar instancias. Para mitigar este tipo de error de implementación, intente implementar las instancias con una configuración de menos recursos, o pruebe a realizar la implementación en un momento posterior.

Para obtener información sobre las cuotas y otros límites en las implementaciones, vea [Cuotas y límites de Azure Container Instances](container-instances-quotas.md).

## <a name="availability---general"></a>Disponibilidad: general

| Ubicación | SO | CPU | Memoria (GB) |
| -------- | -- | :---: | :-----------: |
| Centro de Canadá, Centro de EE. UU., Este de EE. UU. 2, Centro-sur de EE. UU. | Linux | 4 | 16 |
| Este de EE. UU., Europa del Norte, Europa Occidental, Oeste de EE. UU., Oeste de EE. UU. 2 | Linux | 4 | 14 |
| Este de Japón | Linux | 2 | 8 |
| Este de Australia, Sudeste Asiático | Linux | 2 | 7 |
| Centro de la India, Asia Oriental, Centro-norte de EE. UU., India meridional | Linux | 2 | 3,5 |
| Este de EE. UU., Europa Occidental, Oeste de EE. UU. |  Windows | 4 | 14 |
| Este de Australia, Centro de Canadá, Centro de la India, Centro de EE. UU., Asia Oriental, Este de EE. UU. 2, Japón Oriental, Centro-norte de EE. UU., Europa del Norte, Centro-sur de EE. UU., India meridional, Sudeste Asiático, Oeste de EE. UU. 2 |  Windows | 2 | 3,5 |

## <a name="availability---virtual-network-deployment-preview"></a>Disponibilidad: implementación de red virtual (versión preliminar)

Las regiones y los recursos siguientes están disponibles para un grupo de contenedores implementado en una [red virtual de Azure](container-instances-vnet.md) (versión preliminar).

[!INCLUDE [container-instances-vnet-limits](../../includes/container-instances-vnet-limits.md)]

## <a name="availability---gpu-resources-preview"></a>Disponibilidad: recursos de GPU (versión preliminar)

Las regiones y los recursos siguientes están disponibles para un grupo de contenedores implementado con [recursos de GPU](container-instances-gpu.md) (versión preliminar).

[!INCLUDE [container-instances-gpu-regions](../../includes/container-instances-gpu-regions.md)]
[!INCLUDE [container-instances-gpu-limits](../../includes/container-instances-gpu-limits.md)]

## <a name="next-steps"></a>Pasos siguientes

Informe al equipo si le gustaría ver regiones adicionales o una mayor disponibilidad de los recursos en [aka.ms/aci/feedback](https://aka.ms/aci/feedback).

Para obtener información sobre cómo solucionar problemas de la implementación de instancias de contenedor, vea [Solución de problemas en las implementaciones en Azure Container Instances](container-instances-troubleshooting.md).
