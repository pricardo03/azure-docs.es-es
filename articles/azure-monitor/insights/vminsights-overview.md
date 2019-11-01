---
title: ¿Qué es Azure Monitor para VM (versión preliminar)? | Microsoft Docs
description: Azure Monitor para VM es una característica de Azure Monitor que combina la supervisión de rendimiento y de mantenimiento del sistema operativo de la VM de Azure, así como la detección automática de las dependencias y los componentes de aplicación con otros recursos, y asigna la comunicación entre ellos. En este artículo se proporciona información general.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 10/15/2019
ms.openlocfilehash: c6135f3ab90a2002c3cf0c8d26211d66d0c637e8
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72802411"
---
# <a name="what-is-azure-monitor-for-vms-preview"></a>¿Qué es Azure Monitor para VM (versión preliminar)?

Azure Monitor para VM supervisa las máquinas virtuales (VM) y los conjuntos de escalado de máquinas virtuales de Azure. El servicio analiza el rendimiento y el estado de las VM Windows y Linux, y supervisa sus procesos y dependencias en otros recursos y procesos externos. 

Permite supervisar el rendimiento y las dependencias de las aplicaciones en máquinas virtuales que están hospedadas en el entorno local o en otro proveedor de servicios en la nube. Tres características clave ofrecen información detallada:

- **Componentes lógicos de máquinas virtuales de Azure y conjuntos de escalado de máquinas virtuales que ejecutan Windows y Linux**: Se miden con respecto a criterios de mantenimiento configurados previamente, y le avisen cuando se cumple la condición evaluada.  

- **Gráficos de rendimiento de tendencias previamente definidos**: Muestra las principales métricas de rendimiento del sistema operativo de la VM.

- **Mapa de dependencias**: Muestra los componentes interconectados con la VM de varios grupos de recursos y suscripciones.  

Las características se organizan en tres perspectivas:

- Health
- Rendimiento
- Map

>[!NOTE]
>Recientemente [anunciamos cambios](https://azure.microsoft.com/updates/updates-to-azure-monitor-for-virtual-machines-preview-before-general-availability-release/
) que estamos llevando a cabo en la característica de mantenimiento en función de los comentarios que recibimos de los clientes de la versión preliminar pública. Dado el número de cambios que realizaremos, vamos a dejar de ofrecer esta característica a los nuevos clientes. Los clientes existentes pueden seguir usando la característica de mantenimiento. Para más información, consulte las [preguntas frecuentes sobre la disponibilidad general](vminsights-ga-release-faq.md).  

La integración con los registros de Azure Monitor ofrece agregación y filtrado eficaces, y puede analizar la tendencias de los datos a lo largo del tiempo. Dicha supervisión integral de las cargas de trabajo no se puede lograr únicamente con Azure Monitor o Service Map.  

Puede ver estos datos directamente en una sola VM desde la máquina virtual, o puede usar a Azure Monitor para ofrecer una vista agregada de las VM. Esta vista se basa en la perspectiva de la característica:

- **Estado**: VM relacionadas con un grupo de recursos.
- **Asignación** y **Rendimiento**: Máquinas virtuales configuradas para informar a un área de trabajo de Log Analytics específica.

![Perspectiva de conclusiones de la máquina virtual desde Azure Portal](./media/vminsights-overview/vminsights-azmon-directvm-01.png)

Azure Monitor para VM puede ofrecer un rendimiento y disponibilidad predecibles de aplicaciones vitales. Identifica los eventos críticos del sistema operativo, los cuellos de botella de rendimiento y problemas de red. Azure Monitor para VM también puede ayudarle a entender si un problema está relacionado con otras dependencias.  

## <a name="data-usage"></a>Uso de datos

Cuando implementa Azure Monitor para VM, los datos que recopilen sus VM se ingieren y almacenan en Azure Monitor. Las métricas de los criterios de mantenimiento se almacenan en Azure Monitor en una base de datos de series temporales; los datos de rendimiento y dependencia recopilados se almacenan en un área de trabajo de Log Analytics. Según los precios que se publican en la [página de precios de Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/), Azure Monitor para VM se factura según:

- Los datos que se ingieren y almacenan.
- El número de series temporales de métricas de criterios de mantenimiento que se supervisan.
- Las reglas de alerta que se crean.
- Las notificaciones que se envían. 

El tamaño del registro varía en función de las longitudes de cadena de los contadores de rendimiento y puede aumentar con el número de discos lógicos y adaptadores de red asignados a la máquina virtual. Si ya tiene un área de trabajo y está recopilando estos contadores, no se aplicará ningún cargo duplicado. Si ya usa Service Map, el único cambio que verá son los datos de conexión adicionales que se envían a Azure Monitor.

## <a name="next-steps"></a>Pasos siguientes

Para conocer los requisitos y los métodos para habilitar la supervisión de máquinas virtuales, consulte [Implementación de Azure Monitor para VM](vminsights-enable-overview.md).
