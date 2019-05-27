---
title: Supervisar el servidor de procesos de Azure Site Recovery
description: En este artículo se describe cómo supervisar el servidor de procesos de Azure Site Recovery.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 08/24/2019
ms.author: raynew
ms.openlocfilehash: f3ba08bc1d557d53bca2421b5052f496c740b185
ms.sourcegitcommit: e9a46b4d22113655181a3e219d16397367e8492d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65969333"
---
# <a name="monitor-the-process-server"></a>Supervisión del servidor de procesos

En este artículo se describe cómo supervisar la [Site Recovery](site-recovery-overview.md) servidor de procesos.

- El servidor de procesos se usa cuando configuró la recuperación ante desastres de máquinas virtuales de VMware locales y servidores físicos en Azure.
- De forma predeterminada, el servidor de procesos se ejecuta en el servidor de configuración. Se instala de forma predeterminada al implementar el servidor de configuración.
- Si lo desea, a escala y controlar un mayor número de máquinas replicadas y aumente el volumen de tráfico de replicación, puede implementar servidores de procesos adicional, la escalabilidad horizontal.

[Obtenga más información](vmware-physical-azure-config-process-server-overview.md) sobre el rol y la implementación de servidores de procesos.

## <a name="monitoring-overview"></a>Información general de supervisión

Puesto que el servidor de procesos tiene muchas funciones, especialmente en el almacenamiento en caché de los datos replicados, la compresión y la transferencia a Azure, es importante supervisar el estado del servidor de proceso de forma continuada.

Hay una serie de situaciones que normalmente afectan al rendimiento del servidor de proceso. Problemas que afectan al rendimiento tendrá un efecto en cascada en el estado de la máquina virtual, insertar, finalmente, el servidor de procesos y sus máquinas replicadas en un estado crítico. Se incluyen situaciones:

- Gran cantidad de máquinas virtuales de usa un servidor de procesos, acercando o que se superen las limitaciones recomendadas.
- Máquinas virtuales con el servidor de procesos tienen una tasa de renovación elevada.
- Rendimiento de la red entre máquinas virtuales y el servidor de procesos no es suficiente para cargar datos de replicación en el servidor de procesos.
- Rendimiento de la red entre el servidor de procesos y Azure no es suficiente para cargar datos de replicación desde el servidor de procesos en Azure.

Todos estos problemas pueden afectar el objetivo de punto de recuperación (RPO) de máquinas virtuales. 

**¿Por qué?** Dado que la generación de un punto de recuperación para una máquina virtual requiere todos los discos de la máquina virtual para tener un punto común. Si un disco tiene una tasa de renovación elevada, la replicación es lenta o el servidor de procesos no es óptimo, afecta a la eficacia con la que se crean puntos de recuperación.

## <a name="monitor-proactively"></a>Supervisar de forma proactiva

Para evitar problemas con el servidor de procesos, es importante:

- Comprender los requisitos específicos para los servidores de procesos mediante [capacidad y la Guía de ajuste de tamaño](site-recovery-plan-capacity-vmware.md#capacity-considerations)y asegúrese de que se implementan servidores de procesos y se ejecutan según las recomendaciones.
- Supervisar alertas y solucionar problemas cuando se producen, para mantener los servidores de procesos que se ejecuten de forma eficaz.


## <a name="process-server-alerts"></a>Alertas del servidor de procesos

El servidor de procesos genera un número de alertas de estado de resumen en la tabla siguiente.

**Tipo de alerta** | **Detalles**
--- | ---
![Estado correcto][green] | Servidor de procesos está conectado y en buen estado.
![Advertencia][yellow] | Uso de CPU > 80% durante los últimos 15 minutos
![Advertencia][yellow] | Uso de memoria > 80% durante los últimos 15 minutos
![Advertencia][yellow] | Carpeta de caché de espacio libre < 30% durante los últimos 15 minutos
![Advertencia][yellow] | No se estén ejecutando los servicios de servidor de procesos para los últimos 15 minutos
![Fundamental][red] | Uso de CPU > 95% durante los últimos 15 minutos
![Fundamental][red] | > 95% de uso de memoria durante los últimos 15 minutos
![Fundamental][red] | Carpeta de caché de espacio libre < 25% para los últimos 15 minutos
![Fundamental][red] | Ningún latido desde el servidor de procesos durante 15 minutos.

![Clave de la tabla](./media/vmware-physical-azure-monitor-process-server/table-key.png)

> [!NOTE]
> El estado general del servidor de proceso se basa en el peor alerta generada.



## <a name="monitor-process-server-health"></a>Monitor estado del servidor de proceso

Puede supervisar el estado de mantenimiento de los servidores de procesos como sigue: 

1. Para supervisar el estado de replicación y el estado de una máquina replicada y de su servidor de procesos en almacén > **elementos replicados**, haga clic en la máquina que desea supervisar.
2. En **estado de replicación**, puede supervisar el estado de mantenimiento de la máquina virtual. Haga clic en el estado para explorar en profundidad para obtener información detallada del error.

    ![Estado del servidor de proceso en el panel de máquinas virtuales](./media/vmware-physical-azure-monitor-process-server/vm-ps-health.png)

4. En **estado del servidor de proceso**, puede supervisar el estado del servidor de procesos. Explorar en profundidad para obtener más información.

    ![Detalles del servidor de proceso en el panel de máquinas virtuales](./media/vmware-physical-azure-monitor-process-server/ps-summary.png)

5. Estado también se puede supervisar mediante la representación gráfica de la página de la máquina virtual.
    - Un servidor de procesos de escalado horizontal será resaltado en color naranja si hay advertencias asociadas con él y rojo si tiene cualquier problema crítico. 
    - Si el servidor de procesos se está ejecutando en la implementación predeterminada en el servidor de configuración, a continuación, el servidor de configuración se resaltarán en consecuencia.
    - Para explorar en profundidad, haga clic en el servidor de configuración o servidor de procesos. Tenga en cuenta los problemas y las recomendaciones de corrección.

También puede supervisar servidores en el almacén en de procesos **infraestructura de Site Recovery**. En **administrar su infraestructura de Site Recovery**, haga clic en **servidores de configuración**. Seleccione el servidor de configuración asociado con el servidor de procesos y la exploración en profundidad hacia abajo en los detalles del servidor de proceso.


## <a name="next-steps"></a>Pasos siguientes

- Si tiene alguno de los problemas de los servidores de proceso, siga nuestro [Guía de solución de problemas](vmware-physical-azure-troubleshoot-process-server.md)
- Si necesita más ayuda, puede publicar su pregunta en el [foro de Azure Site Recovery](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr). 

[green]: ./media/vmware-physical-azure-monitor-process-server/green.png
[yellow]: ./media/vmware-physical-azure-monitor-process-server/yellow.png
[red]: ./media/vmware-physical-azure-monitor-process-server/red.png
