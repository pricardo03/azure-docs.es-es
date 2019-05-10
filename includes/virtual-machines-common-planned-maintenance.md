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
ms.openlocfilehash: adf99b941a775f105d8c65da3ac6c11dc7257120
ms.sourcegitcommit: 6f043a4da4454d5cb673377bb6c4ddd0ed30672d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65416362"
---
Azure actualiza periódicamente la plataforma para mejorar la confiabilidad, el rendimiento y la seguridad de la infraestructura host de las máquinas virtuales. Estas actualizaciones van desde la aplicación de revisiones a componentes de software en el entorno de hospedaje, hasta la retirada de hardware, pasando por la actualización de los componentes de red. La mayoría de estas actualizaciones no afectan a las máquinas virtuales hospedadas. Sin embargo, existen casos en los que las actualizaciones tienen cierto impacto y, por lo tanto, Azure elige el método menos agresivo en las actualizaciones:

- Si es posible realizar una actualización sin reinicio, la máquina virtual se detiene mientras se actualiza el host o se migra directamente a un host ya actualizado.

- Si el mantenimiento requiere un reinicio, el usuario recibe un aviso de cuándo está programado el mantenimiento. Asimismo, Azure le asignará un período de tiempo en el que pueda iniciar el mantenimiento a la hora que le sea más conveniente. Período de tiempo de mantenimiento automático normalmente es de 30 días a menos que sea urgente para realizar tareas de mantenimiento. Azure también está invirtiendo en tecnologías para reducir los casos, cuando las máquinas virtuales deben reiniciarse para el mantenimiento planeado de plataforma. 

En esta página se describe cómo Azure realiza ambos tipos de mantenimiento. Para obtener más información sobre eventos no planeados (interrupciones), consulte Administración de la disponibilidad de las máquinas virtuales para [Windows](../articles/virtual-machines/windows/manage-availability.md) o [Linux](../articles/virtual-machines/linux/manage-availability.md).

Puede obtener una notificación en la máquina virtual sobre el próximo mantenimiento si usa Scheduled Events para [Windows](../articles/virtual-machines/windows/scheduled-events.md) o [Linux](../articles/virtual-machines/linux/scheduled-events.md).

Para obtener información de procedimientos sobre la administración del mantenimiento planeado, vea "Control de las notificaciones de mantenimiento planeadas" para [Linux](../articles/virtual-machines/linux/maintenance-notifications.md) o [Windows](../articles/virtual-machines/windows/maintenance-notifications.md).

## <a name="maintenance-not-requiring-a-reboot"></a>No se requiere un reinicio de mantenimiento

Como se indicó anteriormente, la mayoría de las actualizaciones de plataforma se realizan sin tener impacto en las máquinas virtuales del cliente. Cuando la actualización de cero impacto no es posible Azure elige el mecanismo de actualización es alto impacto mínimo en las máquinas virtuales del cliente. La mayoría de estos mantenimiento impacto distinto de cero hace menos de 10 segundos de pausa de la máquina virtual. En algunos casos, se utilizan mecanismos de mantenimiento de conservación de memoria, lo que pone en pausa la máquina virtual hasta 30 segundos y conserva la memoria RAM. A continuación, se reanuda la máquina virtual y su reloj se sincroniza automáticamente. Mantenimiento con conservación de memoria funciona más del 90% de máquinas virtuales de Azure excepto las series H, M, N y G. Azure usa cada vez más las tecnologías de migración en vivo y mejora el mecanismo de mantenimiento que conserva la memoria para así reducir la duración de la pausa.  

Estas operaciones de mantenimiento sin reinicialización se aplican dominio por dominio, si tienen errores, y el progreso se detiene si se recibe alguna señal de estado de advertencia. 

Algunas aplicaciones pueden resultar afectadas por estos tipos de actualizaciones. Si la máquina virtual se mueve a otro host, puede que algunas cargas de trabajo sensibles sufran una pequeña degradación del rendimiento en los pocos minutos anteriores a la pausa de la máquina virtual. Estas aplicaciones pueden beneficiarse del uso de Scheduled Events para [Windows](../articles/virtual-machines/windows/scheduled-events.md) o [Linux](../articles/virtual-machines/linux/scheduled-events.md), ya que así pueden preparar el mantenimiento de la máquina virtual sin afectar al proceso de mantenimiento de Azure. Azure también está trabajando en características de control de mantenimiento para aplicaciones tan sensibles. 

### <a name="live-migration"></a>Migración en vivo

Migración en vivo es una operación que no sean rebootful que conserva la memoria para la máquina virtual y los resultados en un limitado pausan o la congelación, normalmente no más de 5 segundos de duración. Hoy en día, toda la infraestructura como servicio (IaaS) Virtual Machines, además de la serie H, M, N y G, son aptos para la migración en vivo. Esto equivale a superior al 90% de las máquinas virtuales de IaaS implementado en la flota de Azure. 

Se inicia la migración en vivo mediante el tejido de Azure en los siguientes escenarios:
- Mantenimiento planeado
- Error de hardware
- Optimizaciones de asignación

Migración en vivo se aprovecha en algunos escenarios de mantenimiento planeado, y se puede usar eventos programados para saber de antemano en vivo al inicio de las operaciones de migración.

También se usa la migración en vivo para mover máquinas virtuales fuera de hardware con un error inminente de predicción cuando detecta por nuestros algoritmos de aprendizaje automático y optimizar las asignaciones de la máquina Virtual. Para obtener más información sobre nuestro modelado predictivo que detecta las instancias de hardware degradado, consulte la entrada de blog titulada [resistencia de la máquina Virtual de Azure mejora con la migración en vivo y aprendizaje automático predictivo](https://azure.microsoft.com/blog/improving-azure-virtual-machine-resiliency-with-predictive-ml-and-live-migration/?WT.mc_id=thomasmaurer-blog-thmaure). Los clientes siempre recibirán un aviso de migración en vivo en su portal de Azure en el Monitor o registros de estado del servicio, así como a través de eventos programados si estos se utilizan.

## <a name="maintenance-requiring-a-reboot"></a>Mantenimiento que requiere un reinicio

En el raro caso en que las máquinas virtuales deban reiniciarse para realizar el mantenimiento planeado, recibirá una notificación de antemano. El mantenimiento planeado tiene dos fases: la ventana de autoservicio y una ventana de mantenimiento programado.

La **ventana de autoservicio** le permite iniciar el mantenimiento en sus máquinas virtuales. Durante este tiempo normalmente cuatro semanas, puede consultar cada máquina virtual para ver su estado y comprobar el resultado de la última solicitud de mantenimiento.

Cuando inicia el mantenimiento de autoservicio, su máquina virtual se vuelve a implementar en un nodo ya actualizado. Como la máquina virtual se reinicia, se pierde el disco temporal y se actualizan las direcciones IP dinámicas asociadas con la interfaz de red virtual.

Si inicia el mantenimiento de autoservicio y hay un error durante el proceso, la operación se detiene, la máquina virtual no se actualiza y se le ofrece la posibilidad de reintentar el mantenimiento de autoservicio. 

Cuando ha transcurrido la ventana de autoservicio, comienza la **ventana de mantenimiento programado**. Durante este período de tiempo puede seguir realizando consultas dentro de la ventana de mantenimiento, pero no podrá iniciar el mantenimiento usted mismo.

Para obtener información sobre la administración del mantenimiento que requiere reinicio, consulte "Control de las notificaciones de mantenimiento planeado de máquinas virtuales" [Linux](../articles/virtual-machines/linux/maintenance-notifications.md) o [Windows](../articles/virtual-machines/windows/maintenance-notifications.md). 

### <a name="availability-considerations-during-scheduled-maintenance"></a>Consideraciones sobre disponibilidad durante el mantenimiento programado 

Si decide esperar a la ventana de mantenimiento programado, hay algunas cosas que debe tener en cuenta para mantener la máxima disponibilidad de sus VM. 

#### <a name="paired-regions"></a>Regiones emparejadas

Cada región de Azure se empareja con otra región de la misma zona geográfica, y juntas forman un par de regiones. Durante la fase de mantenimiento programado, Azure solo actualiza las máquinas virtuales en una sola región perteneciente a un par de regiones. Por ejemplo, al actualizar las máquinas virtuales de la zona Centro-norte de EE. UU., Azure no actualizará las máquinas virtuales de Centro-sur de EE. UU. al mismo tiempo. Sin embargo, otras regiones como Europa del Norte pueden estar en mantenimiento al mismo tiempo que el Este de EE. UU. Comprender cómo funcionan los pares de regiones puede ayudar a distribuir mejor las máquinas virtuales entre regiones. Para más información, consulte [Regiones de Azure](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

#### <a name="availability-sets-and-scale-sets"></a>Conjuntos de disponibilidad y conjuntos de escalado

Al implementar una carga de trabajo en máquinas virtuales de Azure, puede crear las máquinas virtuales dentro de un conjunto de disponibilidad para proporcionar alta disponibilidad a su aplicación. Esto garantiza que durante una interrupción o un evento de mantenimiento con una operación de reinicio, al menos haya una máquina virtual disponible.

En un conjunto de disponibilidad, las máquinas virtuales individuales se reparten entre 20 dominios de actualización (UD) como máximo. Durante el mantenimiento planeado, solo un dominio de actualización se ve actualizado en un momento determinado. El orden de los dominios de actualización que se van a actualizar no siempre ocurre de manera secuencial. 

Los conjuntos de escalado de máquinas virtuales son un recurso informático de Azure que permite implementar y administrar un conjunto de máquinas virtuales idénticas como un recurso único. El conjunto de escalado se implementa automáticamente entre dominios de actualización, como las máquinas virtuales de un conjunto de disponibilidad. Igual que sucede con los conjuntos de disponibilidad, con los conjuntos de escalado solo un dominio de actualización se actualiza en un momento determinado durante el mantenimiento programado.

Para obtener más información sobre la configuración de máquinas virtuales para ofrecer alta disponibilidad, consulte Administración de la disponibilidad de las máquinas virtuales para [Windows](../articles/virtual-machines/windows/manage-availability.md) o [Linux](../articles/virtual-machines/linux/manage-availability.md).
