---
title: Disponibilidad de recursos para Azure Container Instances
description: Disponibilidad de recursos de proceso y memoria para el servicio Azure Container Instances en diferentes regiones de Azure.
services: container-instances
author: dlepow
manager: gwallace
ms.service: container-instances
ms.topic: article
ms.date: 05/14/2019
ms.author: danlep
ms.openlocfilehash: 24edce511c2d07050db1e77edeae4e587fcd79b0
ms.sourcegitcommit: ee61ec9b09c8c87e7dfc72ef47175d934e6019cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2019
ms.locfileid: "70172390"
---
# <a name="resource-availability-for-azure-container-instances-in-azure-regions"></a>Disponibilidad de recursos para Azure Container Instances en las regiones de Azure

En este artículo se detalla la disponibilidad de los recursos de proceso y memoria de Azure Container Instances en las regiones de Azure. 

Los valores que se presentan son los recursos máximos disponibles por cada implementación de un [grupo de contenedores](container-instances-container-groups.md). Los valores están actualizados en el momento de la publicación. 

> [!NOTE]
> Los grupos de contenedores creados dentro de estos límites de recursos están sujetos a disponibilidad en la región de implementación. Cuando una región está sometida a mucha carga, puede experimentar un error al implementar instancias. Para mitigar este tipo de error de implementación, intente implementar las instancias con una configuración de menos recursos, o pruebe a realizar la implementación en un momento posterior.

Para obtener información sobre las cuotas y otros límites en las implementaciones, vea [Cuotas y límites de Azure Container Instances](container-instances-quotas.md).

## <a name="availability---general"></a>Disponibilidad: general

Las siguientes regiones y recursos están disponibles para los grupos de contenedores con Linux y [admiten](container-instances-faq.md#what-windows-base-os-images-are-supported) contenedores basados en Windows Server 2016.

| Location | OS | CPU | Memoria (GB) |
| -------- | -- | :---: | :-----------: |
| Centro de Canadá, Centro de la India, Centro de EE. UU., Asia Oriental, Este de EE. UU., Este de EE. UU. 2, Norte de Europa, Centro-sur de EE. UU., Sudeste Asiático, Sur de Reino Unido, Oeste de EE. UU. | Linux | 4 | 16 |
| Oeste de Europa, Oeste de EE. UU. 2 | Linux | 4 | 14 |
| Este de Australia, Japón Oriental | Linux | 2 | 8 |
| Centro y norte de EE. UU., India del Sur | Linux | 2 | 3,5 |
| Europa occidental | Windows | 4 | 16 |
| Este de EE. UU., Oeste de EE. UU. | Windows | 4 | 14 |
| Este de Australia, Centro de Canadá, Centro de la India, Centro de EE. UU., Asia Oriental, Este de EE. UU. 2, Japón Oriental, Centro-norte de EE. UU., Norte de Europa, Centro-sur de EE. UU., Asia Suroriental, India del Sur, Sur de Reino Unido, Oeste de EE. UU. 2 | Windows | 2 | 3,5 |

## <a name="availability---windows-server-2019-ltsc-1809-deployments-preview"></a>Disponibilidad: implementaciones de la versión 1809 para Windows Server 2019 LTSC (versión preliminar)

Las siguientes regiones y recursos están disponibles para los grupos de contenedores con contenedores basados en Windows Server 2019 (versión preliminar).

| Location | OS | CPU | Memoria (GB) |
| -------- | -- | :---: | :-----------: |
| Asia Suroriental, Norte de Europa, Oeste de Europa, Centro de EE. UU., Este de EE. UU., Oeste de EE. UU., Oeste de EE. UU. 2 | Windows | 4 | 16 |
| Este de EE. UU. 2 | Windows | 2 | 3,5 |


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
