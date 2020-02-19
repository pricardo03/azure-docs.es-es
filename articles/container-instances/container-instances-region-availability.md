---
title: Disponibilidad de recursos por región
description: Disponibilidad de recursos de proceso y memoria para el servicio Azure Container Instances en diferentes regiones de Azure.
ms.topic: article
ms.date: 01/31/2020
ms.author: danlep
ms.openlocfilehash: 3f64a13331658c1360c9d54f6f57a68c2c91fc6f
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2020
ms.locfileid: "77117838"
---
# <a name="resource-availability-for-azure-container-instances-in-azure-regions"></a>Disponibilidad de recursos para Azure Container Instances en las regiones de Azure

En este artículo se detalla la disponibilidad de los recursos de proceso, memoria y almacenamiento de Azure Container Instances en las regiones de Azure y del sistema operativo de destino. 

Los valores que se presentan son los recursos máximos disponibles por cada implementación de un [grupo de contenedores](container-instances-container-groups.md). Los valores están actualizados en el momento de la publicación. 

> [!NOTE]
> Los grupos de contenedores creados dentro de estos límites de recursos están sujetos a disponibilidad en la región de implementación. Cuando una región está sometida a mucha carga, puede experimentar un error al implementar instancias. Para mitigar este tipo de error de implementación, intente implementar las instancias con una configuración de menos recursos, o pruebe a realizar la implementación en un momento posterior.

Para obtener información sobre las cuotas y otros límites en las implementaciones, vea [Cuotas y límites de Azure Container Instances](container-instances-quotas.md).

## <a name="availability---general"></a>Disponibilidad: general

Las siguientes regiones y recursos están disponibles para los grupos de contenedores con Linux y [admiten](container-instances-faq.md#what-windows-base-os-images-are-supported) contenedores basados en Windows Server 2016.

| Regions | SO | Uso máximo de CPU | Memoria máxima (GB) | Almacenamiento (GB) |
| -------- | -- | :---: | :-----------: | :---: |
| Sur de Brasil, Centro de Canadá, Centro de la India, Centro de EE. UU., Este de Asia, Este de EE. UU., Este de EE. UU. 2, Norte de Europa, Centro-sur de EE. UU., Sudeste de Asia, Sur de la India, Sur de Reino Unido, Oeste de Europa, Oeste de EE. UU., Oeste de EE. UU. 2 | Linux | 4 | 16 | 50 |
| Este de Australia, Japón Oriental | Linux | 2 | 8 | 50 |
| Centro-Norte de EE. UU | Linux | 2 | 3,5 | 50 |
| Sur de Brasil, Japón Oriental, Oeste de Europa | Windows | 4 | 16 | 20 |
| Este de EE. UU., Oeste de EE. UU. | Windows | 4 | 14 | 20 |
| Este de Australia, Centro de Canadá, Centro de la India, Centro de EE. UU., Este de Asia, Este de EE. UU. 2, Centro-norte de EE. UU., Norte de Europa, Centro-sur de EE. UU., Sudeste de Asia, Sur de la India, Sur de Reino Unido, Oeste de EE. UU. 2 | Windows | 2 | 3,5 | 20 |

## <a name="availability---windows-server-2019-ltsc-1809-deployments-preview"></a>Disponibilidad: implementaciones de la versión 1809 para Windows Server 2019 LTSC (versión preliminar)

Las siguientes regiones y recursos están disponibles para los grupos de contenedores con contenedores basados en Windows Server 2019 (versión preliminar).

| Regions | SO | Uso máximo de CPU | Memoria máxima (GB) | Almacenamiento (GB) |
| -------- | -- | :---: | :-----------: | :---: |
| Este de Australia, Sur de Brasil, Centro de Canadá, Centro de la India, Centro de EE. UU., Este de Asia, Este de EE. UU., Este de Japón, Centro-norte de EE. UU., Norte de Europa, Centro-sur de EE. UU., Sudeste de Asia, Sur de la India, Sur de Reino Unido, Oeste de Europa | Windows | 4 | 16 | 20 |
| Este de EE. UU. 2, Oeste de EE. UU. 2 | Windows | 2 | 3,5 | 20 |


## <a name="availability---virtual-network-deployment"></a>Disponibilidad: implementación de red virtual

Las regiones y los recursos siguientes están disponibles para un grupo de contenedores implementado en una [red virtual de Azure](container-instances-vnet.md).

[!INCLUDE [container-instances-vnet-limits](../../includes/container-instances-vnet-limits.md)]

## <a name="availability---gpu-resources-preview"></a>Disponibilidad: recursos de GPU (versión preliminar)

Las regiones y los recursos siguientes están disponibles para un grupo de contenedores implementado con [recursos de GPU](container-instances-gpu.md) (versión preliminar).

[!INCLUDE [container-instances-gpu-regions](../../includes/container-instances-gpu-regions.md)]
[!INCLUDE [container-instances-gpu-limits](../../includes/container-instances-gpu-limits.md)]

## <a name="next-steps"></a>Pasos siguientes

Informe al equipo si le gustaría ver regiones adicionales o una mayor disponibilidad de los recursos en [aka.ms/aci/feedback](https://aka.ms/aci/feedback).

Para obtener información sobre cómo solucionar problemas de la implementación de instancias de contenedor, vea [Solución de problemas en las implementaciones en Azure Container Instances](container-instances-troubleshooting.md).
