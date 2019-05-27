---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-machines
author: shants123
ms.service: virtual-machines
ms.topic: include
ms.date: 4/30/2019
ms.author: shants
ms.custom: include file
ms.openlocfilehash: c2931fa410cf92755a5df5b7129dcf93de900930
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66155920"
---
Azure actualiza periódicamente su plataforma para mejorar la confiabilidad, rendimiento y seguridad de la infraestructura de host para máquinas virtuales. El propósito de estos intervalos de las actualizaciones de componentes de software en el entorno de hospedaje para actualizar los componentes de red o retirada de hardware de la aplicación de revisiones. 

Actualizaciones rara vez afectan a las máquinas virtuales hospedadas. Cuando las actualizaciones tienen un efecto, Azure elige el método menos impactante para las actualizaciones:

- Si la actualización no requiere un reinicio, la máquina virtual está en pausa mientras se actualiza el host o la máquina virtual está migran en vivo a un host ya actualizado.

- Si el mantenimiento requiere un reinicio, se notifica el mantenimiento planeado. Azure proporciona también un período de tiempo en el que se puede iniciar el mantenimiento usted mismo, cada vez que le convenga. La ventana de mantenimiento de autoservicio normalmente es de 30 días a menos que el mantenimiento es urgente. Azure está invirtiendo en tecnologías para reducir el número de casos en que el mantenimiento planeado de plataforma requiere las máquinas virtuales para reiniciarse. 

En esta página se describe cómo Azure realiza ambos tipos de mantenimiento. Para obtener más información sobre eventos no planeados (interrupciones), consulte [administrar la disponibilidad de las máquinas virtuales para Windows](../articles/virtual-machines/windows/manage-availability.md) o el artículo correspondiente para [Linux](../articles/virtual-machines/linux/manage-availability.md).

Dentro de una máquina virtual, puede obtener notificaciones sobre el mantenimiento próximo por [mediante programado eventos para Windows](../articles/virtual-machines/windows/scheduled-events.md) o para [Linux](../articles/virtual-machines/linux/scheduled-events.md).

Para obtener instrucciones acerca de cómo administrar el mantenimiento planeado, consulte [control planeado las notificaciones de mantenimiento para Linux](../articles/virtual-machines/linux/maintenance-notifications.md) o el artículo correspondiente para [Windows](../articles/virtual-machines/windows/maintenance-notifications.md).

## <a name="maintenance-that-doesnt-require-a-reboot"></a>Mantenimiento que no requiere un reinicio

Como se indicó anteriormente, la mayoría de las actualizaciones de plataforma no afectan a las máquinas virtuales del cliente. Cuando una actualización sin causar impacto no es posible, Azure elige el mecanismo de actualización es alto impacto mínimo en las máquinas virtuales del cliente. 

La mayoría mantenimiento distinto de cero impacto detiene la máquina virtual durante menos de 10 segundos. En algunos casos, Azure usa los mecanismos de mantenimiento de conservación de memoria. Estos mecanismos de la máquina virtual en pausa hasta 30 segundos y conservar la memoria RAM. A continuación, se reanuda la máquina virtual y su reloj se sincroniza automáticamente. 

Mantenimiento de conservación de memoria funciona más del 90 por ciento de máquinas virtuales de Azure. No funciona para la serie H, M, N y G. Cada vez más, Azure usa tecnologías de migración en vivo y mejora los mecanismos de mantenimiento de conservación de memoria para reducir las duraciones de pausa.  

Estas operaciones de mantenimiento que no requieren un reinicio son los dominios de error aplicada a la vez. También se detiene si reciben las señales de estado de advertencia. 

Estos tipos de actualizaciones pueden afectar a algunas aplicaciones. Cuando la máquina virtual se migran en vivo a un host diferente, algunas cargas de trabajo confidenciales podrían mostrar una pequeña degradación del rendimiento en los minutos que hicieron nacer a la pausa de la máquina virtual. Para prepararse para el mantenimiento de la máquina virtual y reducir el impacto durante el mantenimiento de Azure, pruebe [mediante programado eventos para Windows](../articles/virtual-machines/windows/scheduled-events.md) o [Linux](../articles/virtual-machines/linux/scheduled-events.md) para dichas aplicaciones. Azure está trabajando en las características de control de mantenimiento para estas aplicaciones confidenciales. 

### <a name="live-migration"></a>Migración en vivo

Migración en vivo es una operación que no requiere un reinicio y que conserve la memoria para la máquina virtual. Hace una pausa o la inmovilización, normalmente no más de 5 segundos de duración. Excepto G, serie H, M y N, toda la infraestructura como un servicio (IaaS) de las máquinas virtuales, es apta para la migración en vivo. Máquinas virtuales aptas representan más del 90 por ciento de las máquinas virtuales de IaaS que se implementan en la flota de Azure. 

La plataforma Azure inicia la migración en vivo en los escenarios siguientes:
- Mantenimiento planeado
- Error de hardware
- Optimizaciones de asignación

Algunos escenarios de mantenimiento planeado usan migración en vivo, y puede usar eventos programados conocer de antemano cuando en directo se inician las operaciones de migración.

Migración en vivo también se puede usar para mover máquinas virtuales cuando los algoritmos de Azure Machine Learning predicción un error inminente del hardware o cuando desee eliminar las asignaciones de la máquina virtual. Para obtener más información sobre el modelado de predicción que detecta las instancias de hardware degradado, vea [resistencia de máquina virtual de Azure mejora con la migración en vivo y aprendizaje automático predictivo](https://azure.microsoft.com/blog/improving-azure-virtual-machine-resiliency-with-predictive-ml-and-live-migration/?WT.mc_id=thomasmaurer-blog-thmaure). Las notificaciones de migración en vivo aparecen en el portal de Azure en los registros Monitor y el estado del servicio, así como en los eventos programados si utiliza estos servicios.

## <a name="maintenance-that-requires-a-reboot"></a>Mantenimiento que requiere un reinicio

En el caso excepcional donde las máquinas virtuales deben reiniciarse para el mantenimiento planeado, se le notificará con antelación. Mantenimiento planeado tiene dos fases: la fase de autoservicio y una fase de mantenimiento programado.

Durante la *fase autoservicio*, que normalmente tarda cuatro semanas, iniciar el mantenimiento en las máquinas virtuales. Como parte de la característica de autoservicio, puede consultar cada máquina virtual para ver su estado y el resultado de la última solicitud de mantenimiento.

Cuando inicia el mantenimiento de autoservicio, su máquina virtual se vuelve a implementar en un nodo ya actualizado. Dado que la máquina virtual se reinicia, se pierde el disco temporal y se actualizan las direcciones IP dinámicas asociadas con la interfaz de red virtual.

Si se produce un error durante el mantenimiento de autoservicio, la operación se detiene, no se actualiza la máquina virtual y obtener la opción Reintentar el mantenimiento de autoservicio. 

Cuando finaliza la fase de autoservicio, la *fase de mantenimiento programado* comienza. Durante esta fase, puede consultar la fase de mantenimiento, pero no se puede iniciar el mantenimiento.

Para obtener más información sobre la administración de mantenimiento que requiere un reinicio, consulte [control planeado las notificaciones de mantenimiento para Linux](../articles/virtual-machines/linux/maintenance-notifications.md) o el artículo correspondiente para [Windows](../articles/virtual-machines/windows/maintenance-notifications.md). 

### <a name="availability-considerations-during-scheduled-maintenance"></a>Consideraciones sobre disponibilidad durante el mantenimiento programado 

Si decide esperar hasta la fase de mantenimiento programado, hay algunas cosas que debe considerar para mantener la máxima disponibilidad de las máquinas virtuales. 

#### <a name="paired-regions"></a>Regiones emparejadas

Cada región de Azure se empareja con otra región de la misma proximidad geográfica. Juntos, hacen un par de regiones. Durante la fase de mantenimiento programado, Azure actualiza solo las máquinas virtuales en una sola región de un par de regiones. Por ejemplo, al actualizar la máquina virtual en la zona norte Central de Ee.uu., Azure no actualiza todas las máquinas virtuales en el sur de EE. UU al mismo tiempo. Sin embargo, otras regiones como Europa del Norte pueden estar en mantenimiento al mismo tiempo que el Este de EE. UU. Comprender cómo funcionan los pares de regiones puede ayudar a distribuir mejor las máquinas virtuales entre regiones. Para más información, consulte [Regiones de Azure](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

#### <a name="availability-sets-and-scale-sets"></a>Conjuntos de disponibilidad y conjuntos de escalado

Al implementar una carga de trabajo en máquinas virtuales de Azure, puede crear las máquinas virtuales dentro de un *conjunto de disponibilidad* para proporcionar alta disponibilidad a su aplicación. Uso de conjuntos de disponibilidad, puede asegurarse de que, durante una interrupción o mantenimiento de los eventos que requieren un reinicio, al menos una máquina virtual está disponible.

En un conjunto de disponibilidad, las máquinas virtuales individuales se reparten entre 20 dominios de actualización (UD) como máximo. Durante el mantenimiento programado, solo un UD se actualiza en un momento dado. Dominios de actualización necesariamente no se actualizan de forma secuencial. 

Máquina virtual *conjuntos de escalado* son recursos que puede usar para implementar y administrar un conjunto de máquinas virtuales idénticas como un único recurso de proceso de Azure. El conjunto de escalado se implementa automáticamente entre dominios de actualización, como máquinas virtuales en un conjunto de disponibilidad. Como con conjuntos de disponibilidad, al usar conjuntos de escalado, solo UD se actualiza en un momento dado durante el mantenimiento programado.

Para obtener más información acerca de cómo configurar las máquinas virtuales de alta disponibilidad, consulte [administrar la disponibilidad de sus máquinas virtuales para Windows](../articles/virtual-machines/windows/manage-availability.md) o el artículo correspondiente para [Linux](../articles/virtual-machines/linux/manage-availability.md).
