---
title: Actualización de Azure Monitor para el agente de contenedores (versión preliminar) | Microsoft Docs
description: En este artículo se describe cómo actualizar al agente de Log Analytics que usa Azure Monitor para los contenedores.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2018
ms.author: magoedte
ms.openlocfilehash: 57bfa47d60ffd8aa7c4240ab77f788773e426bd8
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51714855"
---
# <a name="how-to-upgrade-the-azure-monitor-for-containers-preview-agent"></a>Actualización de Azure Monitor para el agente de contenedores (versión preliminar)
Azure Monitor para contenedores usa una versión con contenedores del agente de Log Analytics para Linux. Cuando se publica una nueva versión del agente, este no se actualiza automáticamente en los clústeres de Kubernetes administrado que se hospedan en Azure Kubernetes Service (AKS).

En este artículo se describe el proceso de actualización del agente.

## <a name="upgrading-agent-on-monitored-kubernetes-cluster"></a>Actualización del agente en un clúster de Kubernetes supervisado
El proceso para actualizar al agente consta de dos pasos sencillos. El primer paso es deshabilitar la supervisión con Azure Monitor para contenedores mediante la CLI de Azure.  Siga los pasos descritos en el artículo [Deshabilitar la supervisión](container-insights-optout.md?toc=%2fazure%2fmonitoring%2ftoc.json#azure-cli). El uso de la CLI de Azure nos permite quitar el agente de los nodos del clúster sin afectar a la solución ni a los datos correspondientes que se almacenan en el área de trabajo. 

>[!NOTE]
>Mientras está realizando esta actividad de mantenimiento, los nodos del clúster no reenviarán los datos recopilados y las vistas de rendimiento no mostrarán datos en el período comprendido entre la eliminación del agente y la instalación de la nueva versión. 
>

Para instalar la nueva versión del agente, siga los pasos descritos en el artículo [Incorporación de la supervisión](container-insights-onboard.md?toc=%2fazure%2fmonitoring%2ftoc.json#enable-monitoring-using-azure-cli) usando la CLI de Azure para completar este proceso.  

Después de volver a habilitar la supervisión, pueden pasar unos 15 minutos hasta que pueda ver actualizada la métrica de estado del clúster. 

## <a name="next-steps"></a>Pasos siguientes
Si experimenta problemas al actualizar el agente, consulte la [guía de solución de problemas](container-insights-troubleshoot.md) para obtener ayuda.