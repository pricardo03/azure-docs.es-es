---
title: Introducción a los registros de recurso de Azure | Microsoft Docs
description: Conozca el esquema de los eventos y servicios admitidos para los registros de recurso de Azure.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: reference
author: rboucher
ms.author: robb
ms.date: 10/22/2019
ms.openlocfilehash: b953f9b5e5fd8c853746caad3047986786bd1317
ms.sourcegitcommit: 92d42c04e0585a353668067910b1a6afaf07c709
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2019
ms.locfileid: "72989623"
---
# <a name="azure-resource-logs-overview"></a>Introducción a los registros de recurso de Azure
Los registros de recurso de Azure son [registros de plataforma](platform-logs-overview.md) emitidos por los recursos de Azure que describen su funcionamiento interno. Todos los registros de recurso comparten un esquema general común, con flexibilidad para que cada servicio emita propiedades únicas para sus propios eventos.

> [!NOTE]
> Los registros de recurso se conocían anteriormente como registros de diagnóstico.

## <a name="collecting-resource-logs"></a>Recopilación de registros de recurso
Los registros de recurso los generan los recursos de Azure compatibles automáticamente, pero no se recopilan a menos que establezca una [configuración de diagnóstico](diagnostic-settings.md). Cree una configuración de diagnóstico para cada recurso de Azure para reenviar los registros a los destinos siguientes:

| Destination | Escenario |
|:---|:---|:---|
| [Área de trabajo de Log Analytics](resource-logs-collect-workspace.md) | Analice los registros con otros datos de supervisión y aproveche las características de Azure Monitor, como las consultas de registro y las alertas de registro. |
| [Almacenamiento de Azure](resource-logs-collect-storage.md) | Archive los registros para la auditoría o la copia de seguridad. |
| [Centro de eventos](resource-logs-stream-event-hubs.md) | Transmita los registros a sistemas de registro y telemetría de terceros.  |

## <a name="compute-resources"></a>Recursos de proceso
Los registros de recurso se diferencian de los de nivel de sistema operativo invitado en los recursos de proceso de Azure. Los recursos de proceso requieren un agente para recopilar los registros y las métricas de su sistema operativo invitado, que incluye datos como los registros de eventos, syslog y el contador de rendimiento. Use la [extensión Diagnostics](agents-overview.md#azure-diagnostic-extension) para enrutar los datos de registro de las máquinas virtuales de Azure y el [agente de Log Analytics](agents-overview.md#log-analytics-agent) para recopilar registros y métricas de las máquinas virtuales en Azure, en otras nubes y en entornos locales en un área de trabajo de Log Analytics. Consulte [Orígenes de datos de supervisión para Azure Monitor](data-sources.md) para más detalles.

## <a name="resource-logs-schema"></a>Esquema de registros de recurso
Para más información sobre el esquema de registros de recursos y las categorías, vea el [Esquema de registro de recursos](diagnostic-logs-schema.md). 

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre otros registros de plataforma de Azure](platform-logs-overview.md) que puede usar para supervisar Azure.
