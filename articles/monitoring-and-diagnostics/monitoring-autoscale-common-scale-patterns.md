---
title: Información general sobre los patrones comunes de escalado automático
description: Obtenga información sobre algunos de los patrones comunes de escalado automático de recursos en Azure.
author: anirudhcavale
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 05/07/2017
ms.author: ancav
ms.component: autoscale
ms.openlocfilehash: 84727ec3694f64d40ad002a248a255df9074d7f4
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35263270"
---
# <a name="overview-of-common-autoscale-patterns"></a>Información general sobre los patrones comunes de escalado automático
En este artículo se describen algunos de los patrones comunes para escalar recursos en Azure.

El escalado automático de Azure Monitor solo se aplica a Conjuntos de escalado de máquinas virtuales, Cloud Services, planes de App Service y App Service Environment. 

# <a name="lets-get-started"></a>Introducción

En este artículo se asume que está familiarizado con el escalado automático. Puede [comenzar aquí a escalar los recursos][1]. A continuación, se indican algunos de los patrones comunes de escalado.

## <a name="scale-based-on-cpu"></a>Escala en función de la CPU

Tiene una aplicación web (rol de VMSS o de servicio en la nube) y: 

- Desea escalar o reducir horizontalmente en función de la CPU.
- Además, desea asegurarse de que hay un número mínimo de instancias. 
- También quiere asegurarse de que establece un límite máximo del número de instancias al que puede escalar.

![Escala en función de la CPU][2]

## <a name="scale-differently-on-weekdays-vs-weekends"></a>Escalado distinto entre los días de la semana y los fines de semana

Tiene una aplicación web (rol de VMSS o de servicio en la nube) y:

- Desea 3 instancias de forma predeterminada (en días de la semana).
- No espera que haya tráfico durante los fines de semana y, por tanto, desea reducir verticalmente a 1 instancia para los fines de semana.

![Escalado distinto entre los días de la semana y los fines de semana][3]

## <a name="scale-differently-during-holidays"></a>Escalado distinto durante festividades

Tiene una aplicación web (rol de VMSS o de servicio en la nube) y: 

- Desea escalar o reducir verticalmente en función del uso de CPU de forma predeterminada.
- Sin embargo, durante Navidad u otros días específicos importantes para su negocio, desea reemplazar los valores predeterminados y disponer de más capacidad.

![Escalado distinto durante festividades][4]

## <a name="scale-based-on-custom-metric"></a>Escalado en función de métricas personalizadas

Tiene un front-end web y un nivel de API que se comunica con el back-end. 

- Desea escalar el nivel de API en función de eventos personalizados en el front-end; por ejemplo, desea escalar el proceso de compra en función del número de artículos que hay en el carro de la compra.

![Escalado en función de métricas personalizadas][5]

<!--Reference-->
[1]: ./monitoring-autoscale-get-started.md
[2]: ./media/monitoring-autoscale-common-scale-patterns/scale-based-on-cpu.png
[3]: ./media/monitoring-autoscale-common-scale-patterns/weekday-weekend-scale.png
[4]: ./media/monitoring-autoscale-common-scale-patterns/holidays-scale.png
[5]: ./media/monitoring-autoscale-common-scale-patterns/custom-metric-scale.png