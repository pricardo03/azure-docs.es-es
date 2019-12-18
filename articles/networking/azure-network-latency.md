---
title: Estadísticas de latencia de ida y vuelta de red de Azure | Microsoft Docs
description: Obtenga información sobre las estadísticas de latencia de ida y vuelta entre regiones de Azure.
services: networking
author: nayak-mahesh
ms.service: virtual-network
ms.topic: article
ms.date: 12/04/2019
ms.author: mnayak
ms.openlocfilehash: 3947df81b67d5aefc1b628b6ddaf8275152a4cd3
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2019
ms.locfileid: "74893099"
---
# <a name="azure-network-round-trip-latency-statistics"></a>Estadísticas de latencia de ida y vuelta de red de Azure

Azure supervisa continuamente la latencia (velocidad) de las áreas principales de su red mediante herramientas de supervisión internas, así como las mediciones recopiladas por [ThousandEyes](https://thousandeyes.com), un servicio de supervisión sintética de terceros.

## <a name="how-are-the-measurements-collected"></a>¿Cómo se recopilan las medidas?

Las medidas de latencia se recopilan de agentes de ThousandEyes hospedados en regiones en la nube de Azure de todo el mundo, que envían continuamente sondeos de red entre ellos, en intervalos de 1 minuto. Las estadísticas de latencia mensuales se derivan de calcular el promedio de las muestras recopiladas del mes.

## <a name="november-2019-latency-figures"></a>Cifras de latencia de noviembre de 2019

**Actualización de noviembre:** Se han agregado 3 regiones.

* Este de Noruega
* Oeste de Noruega
* Australia

A continuación se muestra el promedio mensual de los tiempos de ida y vuelta entre las regiones de Azure durante los últimos 30 días (que terminan el 30 de noviembre de 2019). Las siguientes medidas cuentan con la tecnología de [ThousandEyes](https://thousandeyes.com).

![Estadísticas de latencia interregionales de Azure](media/azure-network-latency/latency-nov-2019.png)

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las [regiones de Azure](https://azure.microsoft.com/global-infrastructure/regions/).
