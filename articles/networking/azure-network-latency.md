---
title: Estadísticas de latencia de ida y vuelta de red de Azure | Microsoft Docs
description: Obtenga información sobre las estadísticas de latencia de ida y vuelta entre regiones de Azure.
services: networking
documentationcenter: na
author: nayak-mahesh
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/04/2019
ms.author: mnayak
ms.openlocfilehash: 500676983233f943fdc9638d75758645dee65564
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73586873"
---
# <a name="azure-network-round-trip-latency-statistics"></a>Estadísticas de latencia de ida y vuelta de red de Azure

Azure supervisa continuamente la latencia (velocidad) de las áreas principales de su red mediante herramientas de supervisión internas, así como las mediciones recopiladas por [ThousandEyes](https://thousandeyes.com), un servicio de supervisión sintética de terceros.

## <a name="how-are-the-measurements-collected"></a>¿Cómo se recopilan las medidas?

Las medidas de latencia se recopilan de agentes de ThousandEyes hospedados en regiones en la nube de Azure de todo el mundo, que envían continuamente sondeos de red entre ellos, en intervalos de 1 minuto. Las estadísticas de latencia mensuales se derivan de calcular el promedio de las muestras recopiladas del mes.

## <a name="october-2019-latency-figures"></a>Cifras de latencia de octubre de 2019

Durante los 31 días finales del 31 de octubre de 2019, los tiempos de latencia de ida y vuelta mensuales mínimos y máximos en las regiones agregadas son:

- De **5 ms** a **72 ms** para la ida y vuelta en regiones de **Norteamérica**.
- De **3 ms** a **28 ms** para la ida y vuelta en regiones de **Europa**.
- De **4 ms** a **134 ms** para la ida y vuelta en regiones de **Asia**.

Las siguientes medidas de latencia interregionales cuentan con la tecnología de [ThousandEyes](https://thousandeyes.com). La unidad de medida de la tabla siguiente es el milisegundo (ms).

![Estadísticas de latencia interregionales de Azure](media/azure-network-latency/azure-inter-region-latency.png)


## <a name="next-steps"></a>Pasos siguientes
- Más información sobre las [regiones de Azure](https://azure.microsoft.com/global-infrastructure/regions/).