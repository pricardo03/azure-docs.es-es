---
title: ¿Qué es Azure Monitor para máquinas virtuales? | Microsoft Docs
description: Azure Monitor para VM es una característica de Azure Monitor que combina la supervisión de rendimiento y de mantenimiento del sistema operativo de la VM de Azure, así como la detección automática de las dependencias y los componentes de aplicación con otros recursos, y asigna la comunicación entre ellos. En este artículo se proporciona información general.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: magoedte
ms.openlocfilehash: 79f507c342f5a13c4d3784cf312f0bf8aeffa3e3
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46957265"
---
## <a name="what-is-azure-monitor-for-vms"></a>¿Qué es Azure Monitor para máquinas virtuales?

La característica Azure Monitor para VM supervisa las máquinas virtuales (VM) de Azure a escala y analiza el rendimiento y el estado de las máquinas virtuales de Windows y Linux, incluidos los diferentes procesos y las dependencias interconectadas con otros recursos y con procesos externos. La solución permite supervisar el rendimiento y las dependencias de las aplicaciones de máquinas virtuales hospedadas en el entorno local o en otro proveedor en la nube.  Incluye tres características clave para ofrecer esta información detallada:

* Los componentes lógicos de las VM de Azure que ejecutan el sistema operativo Windows y Linux se miden en función de un conjunto de criterios de estado configurados previamente y de alertas cuando se cumple la condición evaluada.  
* Las métricas de rendimiento de núcleos del procesador, la memoria, el disco y el adaptador de red del sistema operativo de VM de invitado se recopilan y se presentan en gráficos de rendimiento de tendencias predefinidos.
* Mapa de dependencias que muestra los componentes interconectados detectados con esa VM desde varios grupo de recursos y suscripciones.  

Estas características se organizan en tres perspectivas:

* Health
* Rendimiento
* Map

>[!NOTE]
>Actualmente, solo se ofrece la característica de mantenimiento para las máquinas virtuales de Azure.
>

La integración con Log Analytics ofrece agregación, filtrado y capacidad de desempeño eficaces de análisis de tendencias de los datos a lo largo del tiempo. La supervisión integral de las cargas de trabajo no se puede lograr únicamente con Azure Monitor, Service Map o Log Analytics.  

Puede ver estos datos directamente en el contexto de la VM única de la máquina virtual, o bien Azure Monitor ofrece una vista agregada de las VM según la perspectiva siguiente de cada característica:

* Estado: VM relacionadas con un grupo de recursos
* Asignación y rendimiento: VM configuradas para informar a un área de trabajo específica de Log Analytics

![Perspectiva de conclusiones de la máquina virtual desde el portal](./media/monitoring-vminsights-overview/vminsights-azmon-directvm-01.png)

DevOps puede ofrecer de forma eficaz un rendimiento predecible y disponibilidad de las aplicaciones vitales mediante la identificación de eventos críticos del sistema operativo y cuellos de botella de rendimiento, proporcionar problemas de red, así como comprender si un problema está relacionado con otras dependencias.  

## <a name="data-usage"></a>Uso de datos 

Tan pronto como incorpore Azure Monitor para las VM, los datos que recopilen sus VM se ingieren y almacenan en Azure Monitor.  Azure Monitor para máquinas virtuales se factura según los datos ingeridos y retenidos, el número de series temporales de métricas de criterios de estado supervisadas, las reglas de alertas creadas y las notificaciones enviadas, a partir de los precios publicados en la [página de precios](https://azure.microsoft.com/pricing/details/monitor/) de Azure Monitor.

El tamaño del registro varía en función de las longitudes de cadena de los contadores y puede aumentar con el número de discos lógicos y adaptadores de red.  Si ya tiene un área de trabajo y está recopilando estos contadores, no se aplicará ningún cargos duplicado.  Si ya usa Service Map, el único cambio que verá son los datos de conexión adicionales que se envían a Azure Monitor.

## <a name="next-steps"></a>Pasos siguientes
Consulte el artículo sobre la [incorporación de Azure Monitor para las máquinas virtuales](monitoring-vminsights-onboard.md) para empezar a supervisar sus máquinas virtuales de Azure.
