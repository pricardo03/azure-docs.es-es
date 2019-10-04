---
title: Registros y métricas de Azure | Microsoft Docs
description: Información general sobre los registros de diagnóstico de Azure que proporciona datos exhaustivos y frecuentes sobre el funcionamiento de un recurso de Azure.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: bwren
ms.subservice: logs
ms.openlocfilehash: 3fe220506e074f881a16c1805d25fb4b39927488
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2019
ms.locfileid: "71262979"
---
# <a name="logs-and-metrics-in-azure"></a>Registros y métricas de Azure
Existen diferentes [registros](data-platform-logs.md) y [métricas](data-platform-metrics.md).

Las aplicaciones y los servicios de supervisión de Azure se pueden dividir en métricas y registros que se almacenan en la [plataforma de datos de Azure](data-platform.md). 

Los registros y las métricas se pueden dividir en dos categorías.

- Plataforma: registros y métricas que se generan automáticamente sin necesidad de ninguna configuración que no sea simplemente 



| Nivel | Registros de plataforma | Métricas de plataforma | Registros personalizados | Métricas personalizadas |
|:---|:---|:---|:---|:---|
| Application  | | | | |
| SO invitado     | Latido |  | Extensión Diagnostics<br>Agente de Log Analytics | Extensión Diagnostics |
| Resource     | [Registros de recursos](resource-logs-overview.md)<br>(específico de cada servicio) | [Métricas de recursos](metrics-supported.md)<br>(específico de cada servicio) | | [Métricas personalizadas](metrics-custom-overview.md) |
| Subscription | [Registro de actividad](activity-logs-overview.md) | | | |
| Inquilino       | 

## <a name="next-steps"></a>Pasos siguientes

* [Transmisión de registros de diagnóstico a **Event Hubs**](resource-logs-stream-event-hubs.md)
* [Cambio de la configuración de diagnóstico de recursos con la API de REST de Azure Monitor](https://docs.microsoft.com/rest/api/monitor/)
* [Análisis de registros desde Azure Storage con Azure Monitor](collect-azure-metrics-logs.md)
