---
title: Optimizar el rendimiento en las máquinas virtuales de serie Lsv2 Azure - Storage | Microsoft Docs
description: Obtenga información sobre cómo optimizar el rendimiento de la solución en las máquinas virtuales de serie Lsv2.
services: virtual-machines-windows
author: laurenhughes
manager: jeconnoc
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 04/17/2019
ms.author: joelpell
ms.openlocfilehash: f84e81a5a9e9c9cf6f477adefa0869d776f7dd71
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "60014792"
---
# <a name="optimize-performance-on-the-lsv2-series-virtual-machines"></a>Optimizar el rendimiento en las máquinas virtuales de serie Lsv2

Máquinas virtuales de serie Lsv2 admiten una variedad de cargas de trabajo que necesitan alta E/S y el rendimiento en el almacenamiento local en una amplia gama de sectores y aplicaciones.  La serie Lsv2 es ideal para los datos de gran tamaño, SQL, NoSQL bases de datos, almacenamiento de datos y grandes bases de datos transaccionales, incluyendo Cassandra, MongoDB, Cloudera y Redis.

El diseño de las máquinas virtuales de serie Lsv2 (VM) maximiza el procesador AMD EPYC™ 7551 para proporcionar el mejor rendimiento entre el procesador, memoria, las máquinas virtuales y dispositivos NVMe. Además de maximizar el rendimiento de hardware, las máquinas virtuales de serie Lsv2 están diseñadas para funcionar con las necesidades de los sistemas operativos Windows y Linux para mejorar el rendimiento con el hardware y el software.

Ajuste el hardware y software dieron lugar a la versión optimizada de [2019 de Windows Server Datacenter](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WindowsServer?tab=Overview), que se lanzó en principios de diciembre de 2018 en Azure Marketplace, que es compatible con un rendimiento máximo en los dispositivos NVMe en serie Lsv2 Máquinas virtuales.

En este artículo proporciona consejos y sugerencias para asegurarse de las aplicaciones y cargas de trabajo de lograr el máximo rendimiento diseñado en las máquinas virtuales. La información de esta página se actualizará continuamente cuando se agregan más Lsv2 optimizado para imágenes a Azure Marketplace.

## <a name="amd-eypc-chipset-architecture"></a>Arquitectura de conjunto de chips AMD EYPC™

Máquinas virtuales de serie Lsv2 usan AMD EYPC™ procesadores de servidor basados en la microarquitectura Zen. AMD desarrollado Fabric (si es infinito) para EYPC™ como escalable para su modelo NUMA que podría usarse para las comunicaciones en la placa, en el paquete y varios paquetes de interconexión. En comparación con QPI (interconexión de ruta de acceso rápido) y el impago de artículos (interconexión de Ultra-Path) utilizado en los procesadores de Intel modernas monolíticas-chip, arquitectura de chip pequeño de AMD varios NUMA puede traer ambas ventajas de rendimiento, así como los desafíos. El impacto real de restricciones de ancho de banda y latencia de memoria puede variar según el tipo de cargas de trabajo.

## <a name="tips-for-maximizing-performance"></a>Sugerencias para maximizar el rendimiento

* El hardware que se utiliza en las máquinas virtuales de serie Lsv2 utiliza dispositivos NVMe con ocho s pares de cola de E/S (QP). Cada cola de E/S de dispositivos NVMe es realmente un par: una cola de envío y una cola de finalización. El controlador de NVMe está configurado para optimizar el uso de estas ocho QPs E/S distribuyendo puedo y programación O en una operación por turnos. Para obtener un rendimiento máximo, ejecutar trabajos de ocho por dispositivo para que coincida con.

* Evite mezclar NVMe de los comandos de administrador (por ejemplo, consulta info NVMe inteligente, etc.) con los comandos de E/S de NVMe durante cargas de trabajo activas. Dispositivos NVMe Lsv2 están respaldados por la tecnología Hyper-V NVMe Direct, que se activa en "modo de baja velocidad" cada vez que los comandos del Administrador de NVMe están pendientes. Los usuarios de Lsv2 podrían ver considerablemente el rendimiento drop en el rendimiento de E/S de NVMe si esto sucede.

* Lsv2 a los usuarios no deben depender de la información de NUMA de dispositivo (0) notificada desde la máquina virtual para las unidades de datos para decidir la afinidad NUMA para sus aplicaciones. La manera recomendada para mejorar el rendimiento es distribuir las cargas de trabajo en las CPU si es posible. 

* La profundidad máxima de cola admitidos por cada par de cola de E/S de dispositivo Lsv2 NVMe de máquina virtual es 1024 (frente a Amazon i3 QD 32 límite). Los usuarios de Lsv2 deben limitar sus cargas de trabajo de pruebas comparativas (sintéticos) a la profundidad de cola 1024 o más abajo para evitar desencadenar condiciones completos de cola, que pueden reducir el rendimiento.

## <a name="utilizing-local-nvme-storage"></a>Uso de almacenamiento local de NVMe

Almacenamiento local en el disco NVMe TB 1,92 todas las máquinas virtuales de Lsv2 es efímera. Durante un reinicio estándar correcta de la máquina virtual, se conservarán los datos en el disco local de NVMe. No se conservarán los datos en el NVMe si se vuelve a implementar, se anulará la asignación o se elimina la máquina virtual. No se conservarán los datos si algún otro problema hace que la máquina virtual o el hardware que se está ejecutando, a un estado incorrecto. Cuando esto sucede, se borran de forma segura los datos en el host antiguo.

También habrá casos cuando la máquina virtual debe moverse a una máquina de host diferente, por ejemplo, durante una operación de mantenimiento planeado. Algunos errores de hardware y las operaciones de mantenimiento planeado se pueden anticipar con [Scheduled Events](scheduled-events.md). Eventos programados deben usarse para mantenerse en todas las operaciones de recuperación y mantenimiento prevista.

En el caso de que un evento de mantenimiento planeado requiere que la máquina virtual se vuelve a crear en un nuevo host vacío discos locales, los datos se deben volver a sincronizar (de nuevo, con los datos en el host antiguo que se borra de forma segura). Esto ocurre porque las máquinas virtuales de serie Lsv2 no admiten actualmente la migración en vivo en el disco local de NVMe.

Existen dos modos para el mantenimiento planeado.

### <a name="standard-vm-customer-controlled-maintenance"></a>Mantenimiento controlado por el cliente de máquina virtual estándar

- La máquina virtual se mueve a un host actualizado durante un período de 30 días.
- Datos de almacenamiento local Lsv2 podrían perderse, por lo que se recomienda realizar copias de seguridad datos antes del evento.

### <a name="automatic-maintenance"></a>Mantenimiento automático

- Se produce si el cliente no ejecuta mantenimiento controlado por el cliente o en el caso de los procedimientos de emergencias como un evento de seguridad de día cero.
- Diseñado para conservar los datos del cliente, pero hay un pequeño riesgo de una inmovilización de la máquina virtual o un reinicio.
- Datos de almacenamiento local Lsv2 podrían perderse, por lo que se recomienda realizar copias de seguridad datos antes del evento.

Los próximos eventos de servicio, utilice el proceso de mantenimiento controlado para seleccionar un momento más conveniente para la actualización. Antes del evento, puede realizar copias de seguridad de los datos de premium storage. Una vez completado el evento de mantenimiento, puede devolver los datos en el almacenamiento de NVMe local de máquinas virtuales Lsv2 actualizado.

Los escenarios que mantienen los datos en discos NVMe locales incluyen:

- La máquina virtual se está ejecutando y en buen estado.
- La máquina virtual se reinicia en su lugar (por usted o Azure).
- La máquina virtual se pausa (detuvo sin anulación de la asignación).
- La mayoría de la operaciones de servicio de mantenimiento planeado.

Los escenarios que se borrarán de forma segura los datos para protegerlos del cliente incluyen:

- Se vuelve a implementar la máquina virtual, detiene (desasigna), o elimine ().
- La máquina virtual pasa a ser incorrecta y tiene al servicio se reparan a otro nodo debido a un problema de hardware.
- Un número pequeño del mantenimiento planeado de las operaciones de servicio que requiere que la máquina virtual que reasignarse a otro host para el servicio.

Para obtener más información acerca de las opciones de copia de datos en el almacenamiento local, vea [copia de seguridad y recuperación ante desastres para discos IaaS de Azure](backup-and-disaster-recovery-for-azure-iaas-disks.md).

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

* **¿Cómo se puede iniciar la implementación de máquinas virtuales de serie Lsv2?**  
   Mucho como cualquier otra máquina virtual, use el [Portal](quick-create-portal.md), [CLI de Azure](quick-create-cli.md), o [PowerShell](quick-create-powershell.md) para crear una máquina virtual.

* **¿Un único error de disco NVMe hará que todas las máquinas virtuales en el host para producir un error?**  
   Si se detecta un error de disco en el nodo de hardware, el hardware está en estado de error. Cuando esto ocurre, todas las máquinas virtuales en el nodo se anulará la asignación automáticamente y se mueve a un nodo en buen estado. Las máquinas virtuales de serie Lsv2, esto significa que los datos del cliente en el nodo donde se borran también de forma segura y será necesario volver a crear el cliente en el nuevo nodo. Como se indicó, antes de la migración en vivo está disponible en Lsv2, los datos en el nodo donde se moverán proactivamente con máquinas virtuales de la medida que se transfieren a otro nodo.

* **¿Es necesario realizar ajustes de sondeo en Windows en Windows Server 2012 o Windows Server 2016?**  
   NVMe sondeo solo está disponible en Windows Server 2019 en Azure.  

* **¿Puedo cambiar a un modelo de (ISR) tradicional interrupción rutina de servicio?**  
   Máquinas virtuales de serie Lsv2 están optimizadas para NVMe de sondeo. Las actualizaciones se proporcionan continuamente para mejorar el rendimiento de sondeo.

* **¿Se puede ajustar la configuración de sondeo en Windows Server 2019?**  
   La configuración de sondeo no es ajustable por el usuario.
   
## <a name="next-steps"></a>Pasos siguientes

* Consulte las especificaciones para todos los [optimizadas para el rendimiento de almacenamiento de las máquinas virtuales](sizes-storage.md) en Azure
