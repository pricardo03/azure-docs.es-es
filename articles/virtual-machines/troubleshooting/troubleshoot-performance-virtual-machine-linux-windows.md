---
title: Solución de problemas genéricos de rendimiento para máquinas virtuales de Azure con Linux o Windows
description: En este artículo se describe la solución de problemas genéricos de rendimiento de máquinas virtuales (VM) mediante la supervisión y la observación de los cuellos de botella, y se proporcionan posibles correcciones para las incidencias que se pueden producir.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: top-support-issue, azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 09/18/2019
ms.author: v-miegge
ms.openlocfilehash: fc8cc4834997033203376cd33670cc907e2911e7
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2019
ms.locfileid: "72170298"
---
# <a name="generic-performance-troubleshooting-for-azure-virtual-machine-running-linux-or-windows"></a>Solución de problemas de rendimiento genérico para una máquina virtual de Azure con Linux o Windows

En este artículo se describe la solución de problemas genéricos de rendimiento de máquinas virtuales (VM) mediante la supervisión y la observación de los cuellos de botella, y se proporcionan posibles correcciones para las incidencias que se pueden producir.

## <a name="enabling-monitoring"></a>Habilitación de la supervisión

### <a name="azure-iaas-virtual-machine-monitoring"></a>Supervisión de máquinas virtuales IaaS de Azure

Para supervisar la máquina virtual invitada, use la supervisión de máquinas virtuales de Azure, que le avisará sobre ciertas condiciones de recursos de alto nivel. Para comprobar si tiene habilitados los diagnósticos de máquina virtual, consulte [Información general sobre los registros de recursos de Azure](https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-overview#collecting-resource-logs). Si ve lo siguiente, lo más probable es que no tenga habilitados los diagnósticos:

![La supervisión no está habilitada](media/troubleshoot-performance-virtual-machine-linux-windows/1-virtual-machines-monitoring-not-enabled.png)
 
### <a name="enable-vm-diagnostics-through-microsoft-azure-portal"></a>Habilitación de los diagnósticos de máquina virtual mediante Azure Portal

Para habilitar los diagnósticos de máquina virtual, vaya a la máquina virtual, haga clic en **Configuración** y, luego, en **Diagnósticos**.

![Hacer clic en Configuración y luego en Diagnósticos](media/troubleshoot-performance-virtual-machine-linux-windows/2-virtual-machines-diagnostics.png)
 
### <a name="enable-storage-account-diagnostics-through-azure-portal"></a>Habilitación de los diagnósticos de la cuenta de almacenamiento mediante Azure Portal

En primer lugar, seleccione la máquina virtual para identificar cuál es la cuenta (o cuentas) que usa. Haga clic en **Configuración** y después en **Discos**.

![Hacer clic en Configuración y luego en Discos](media/troubleshoot-performance-virtual-machine-linux-windows/3-storage-disks-disks-selection.png)

En el portal, vaya a la cuenta (o cuentas) de almacenamiento de la máquina virtual y siga estos pasos:

![Seleccionar métricas de blob](media/troubleshoot-performance-virtual-machine-linux-windows/4-select-blob-metrics.png)
 
1. Seleccione **Toda la configuración**.
2. Active Diagnósticos.
3. Seleccione **Blob* Métricas** y establezca la retención en **30** días.
4. Guarde los cambios.

## <a name="observing-bottlenecks"></a>Observación de los cuellos de botella

### <a name="accessing-the-monitoring"></a>Acceso a la supervisión

Seleccione la máquina virtual de Azure que quiere investigar y elija **Supervisión**.

![Selección de Supervisión](media/troubleshoot-performance-virtual-machine-linux-windows/5-observe-monitoring.png)
 
### <a name="timelines-of-observation"></a>Escalas de tiempo de observación

Para identificar si hay cuellos de botella en los recursos, revise los datos. Si la máquina ha estado funcionando bien, pero se ha notificado que el rendimiento ha disminuido recientemente, revise un intervalo de tiempo de datos que abarque los datos de las métricas de rendimiento antes de que se notificara el cambio y durante y después del problema.

### <a name="check-for-cpu-bottleneck"></a>Comprobación de los cuellos de botella en la CPU

![Comprobación de los cuellos de botella en la CPU](media/troubleshoot-performance-virtual-machine-linux-windows/6-cpu-bottleneck-time-range.png)

1. Edite el gráfico.
2. Defina el intervalo de tiempo.
3. A continuación, debe agregar el contador: CPU Percentage Guest OS (Porcentaje de CPU: SO invitado)
4. Haga clic en Save (Guardar).

### <a name="cpu-observe-trends"></a>Tendencias de observación de la CPU

Al examinar los problemas de rendimiento, debe tener en cuenta las tendencias y comprender si le afectan. En las secciones siguientes, usaremos los gráficos de supervisión del portal para mostrar las tendencias. También pueden ser útiles para crear referencias cruzadas de las diferencias entre los comportamientos de los recursos en el mismo período de tiempo. Para personalizar los gráficos, haga clic en [Plataforma de datos de Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform).

Subiendo: los picos podrían estar relacionados con una tarea programada o un evento conocido. Si puede identificar la tarea, determine si esta se ejecuta con el nivel de rendimiento necesario. Si el rendimiento es aceptable, puede que no necesite aumentar los recursos.

Máximo y constante: con frecuencia indica una nueva carga de trabajo. Si no es una carga de trabajo reconocida, habilite la supervisión en la máquina virtual para averiguar qué proceso (o procesos) provoca el comportamiento. Una vez reconocido el proceso, determine si el aumento del consumo se debe a un código ineficaz o a un consumo normal. Si el consumo es normal, decida si el proceso funciona con el nivel de rendimiento necesario.

Constante: determine si la máquina virtual se ha ejecutado siempre en este nivel, o si solo lo ha hecho desde que se han habilitado los diagnósticos. En caso afirmativo, identifique el proceso (o procesos) que causa el problema y considere la posibilidad de agregar más recursos.

Aumento constante: un aumento constante en el consumo suele ser debido a un código ineficaz o un proceso que asume más carga de trabajo de usuario.

### <a name="high-cpu-utilization-remediation"></a>Corrección del uso elevado de la CPU

Si la aplicación o el proceso no se ejecutan con el nivel de rendimiento correcto y observa una utilización constante de la CPU de más del 95 %, puede realizar cualquiera de las siguientes tareas:

* Como solución inmediata: aumente el tamaño de la máquina virtual a uno con más núcleos.
* Comprenda el problema: busque la aplicación o el proceso y solucione los problemas en consecuencia.

Si ha aumentado la máquina virtual y la CPU se sigue ejecutando al 95 %, determine si esta configuración ofrece un mejor rendimiento o una capacidad de proceso de aplicación más elevada hasta un nivel aceptable. Si no es así, solucione los problemas de la aplicación o el proceso.

## <a name="check-for-memory-bottleneck"></a>Comprobación de los cuellos de botella en la memoria

Para ver las métricas:

1. Agregue una sección.
2. Agregue un icono.
3. Abra la galería.
4. Seleccione el uso de memoria y arrastre. Cuando el icono esté acoplado, haga clic con el botón derecho y seleccione **6x4**.

### <a name="memory-observe-trends"></a>Tendencias de observación de la memoria

El uso de memoria muestra cuánta memoria se consume con la máquina virtual. Comprenda la tendencia y si corresponde a la hora en que aparecen los problemas. Siempre debe tener más de 100 MB de memoria disponible.

Máximo y constante/consumo estable constante: puede que el uso elevado de memoria no sea la causa del rendimiento deficiente, puesto que algunas aplicaciones, como los motores de base de datos relacional, asignan una gran cantidad de memoria, y puede que este uso no sea de importancia. Sin embargo, si hay varias aplicaciones que necesiten memoria, puede presenciar un rendimiento deficiente por la contención de memoria, lo que provoca recortes y paginación o intercambio en el disco. Este bajo rendimiento suele ser una causa apreciable del impacto del rendimiento en las aplicaciones.

Consumo cada vez más estable: un posible "calentamiento" de la aplicación; este consumo es común entre motores de base de datos durante el inicio. Sin embargo, también podría ser un indicio de pérdida de memoria en una aplicación. Identifique la aplicación y vea si se trata de un comportamiento esperado.

Uso de archivos de paginación o intercambio: compruebe si está haciendo un uso excesivo del archivo de paginación de Windows (ubicado en D:\) o del archivo de intercambio de Linux (ubicado en `/dev/sdb`). Si no tiene nada en estos volúmenes excepto estos archivos, compruebe si hay numerosas operaciones de lectura y escritura en esos discos. Este problema indica condiciones de memoria insuficiente.

### <a name="high-memory-utilization-remediation"></a>Corrección del uso elevado de memoria

Para resolver los problemas de un uso elevado de memoria, realice cualquiera de las siguientes tareas:

* Para resolver inmediatamente el problema de uso de los archivos de paginación o intercambio: aumente el tamaño de la máquina virtual a una con más memoria y luego realice la supervisión.
* Comprenda el problema: busque las aplicaciones o procesos y realice tareas de solución de problemas a fin de identificar las aplicaciones que consumen más memoria.
* Si conoce la aplicación, vea si se puede limitar la asignación de memoria.

Si después de actualizar a una máquina virtual más grande, descubre que todavía tiene un aumento estable constante de hasta el 100 %, identifique la aplicación o el proceso y solucione los problemas.

## <a name="check-for-disk-bottleneck"></a>Comprobación de los cuellos de botella en el disco

Para comprobar el subsistema de almacenamiento de la máquina virtual, consulte los diagnósticos en el nivel de máquina virtual de Azure mediante el uso de los contadores de diagnósticos de máquina virtual y también los diagnósticos de la cuenta de almacenamiento.

Tenga en cuenta que no existen contadores para cuentas de Premium Storage y con redundancia de zona. En el caso de problemas relacionados con estos contadores, genere una incidencia de soporte técnico.

### <a name="viewing-storage-account-diagnostics-in-monitoring"></a>Visualización de los diagnósticos de la cuenta de almacenamiento en la supervisión

Para trabajar con los siguientes elementos, vaya a la cuenta de almacenamiento de la máquina virtual en el portal:

![Visualización de los diagnósticos de la cuenta de almacenamiento en la supervisión](media/troubleshoot-performance-virtual-machine-linux-windows/7-virtual-machine-storage-account.png)

1. Edite el gráfico de supervisión.
2. Defina el intervalo de tiempo.
3. Agregue los contadores descritos en los pasos siguientes.
4. Guarde los cambios.

### <a name="disk-observe-trends-standard-storage-only"></a>Tendencias de observación del disco (solo almacenamiento estándar)

Para identificar problemas con el almacenamiento, examine las métricas de rendimiento de los diagnósticos de la cuenta de almacenamiento y los diagnósticos de máquina virtual.

En cada una de las siguientes comprobaciones, busque las tendencias clave de cuándo se producen los problemas en el intervalo de tiempo del problema.

#### <a name="check-azure-storage-availability--add-the-storage-account-metric-availability"></a>Comprobar la disponibilidad del almacenamiento de Azure y agregar la métrica de la cuenta de almacenamiento: disponibilidad

Si observa un descenso de la disponibilidad, podría haber un problema con la plataforma; compruebe el [estado de Azure](https://azure.microsoft.com/status/). Si no se muestra ningún problema, genere una nueva solicitud de soporte técnico.

#### <a name="check-for-azure-storage-timeout---add-the-storage-account-metrics"></a>Comprobar el tiempo de espera de almacenamiento de Azure y agregar las métricas de la cuenta de almacenamiento:

* ClientTimeOutError
* ServerTimeOutError
* AverageE2ELatency
* AverageServerLatency
* TotalRequests

Los valores de las métricas *TimeOutError indican que una operación de E/S tardó demasiado y agotó el tiempo de espera. Use los siguientes pasos para identificar posibles causas.

El aumento de AverageServerLatency al mismo tiempo en TimeOutErrors podría deberse a un problema de la plataforma. En este caso, genere una nueva solicitud de soporte técnico.

AverageE2ELatency representa la latencia del cliente. Compruebe cómo la aplicación realiza las operaciones IOPS. Busque un aumento o una métrica TotalRequests constantemente alta. Esta métrica representa las operaciones IOPS. Si está empezando a alcanzar los límites de la cuenta de almacenamiento o de un solo disco duro virtual, la latencia puede estar relacionada con la limitación.

#### <a name="check-for-azure-storage-throttling---add-the-storage-account-metrics-throttlingerror"></a>Comprobar la limitación de Azure Storage y agregar las métricas de la cuenta de almacenamiento: ThrottlingError

Los valores de limitación indican que está experimentando restricciones en el nivel de la cuenta de almacenamiento, lo que significa que se va a alcanzar el límite de IOPS de la cuenta. Para determinar si va a alcanzar el umbral de IOPS, compruebe la métrica **TotalRequests**.

Tenga en cuenta que cada disco duro virtual tiene un límite de 500 IOPS o 60 MBits, pero está sujeto al límite acumulado de 20000 IOPS por cuenta de almacenamiento.

Con esta métrica, no puede saber qué blob es el causante de la limitación y cuáles se ven afectados por ella. Sin embargo, va a alcanzar los límites de IOPS o de entrada y salida de la cuenta de almacenamiento.

Para identificar si va a alcanzar el límite de IOPS, vaya a los diagnósticos de la cuenta de almacenamiento y compruebe la métrica TotalRequests para ver si se aproxima a 20 000 TotalRequests. Identifique cualquier cambio en el patrón: si es la primera vez que ve el límite o si este límite sucede en determinados momentos.

#### <a name="references"></a>Referencias

* [Objetivos de escalabilidad para discos de máquinas virtuales](https://azure.microsoft.com/documentation/articles/storage-scalability-targets/#scalability-targets-for-virtual-machine-disks)

El ancho de banda de la cuenta de almacenamiento se mide mediante las métricas de la cuenta de almacenamiento: TotalIngress y TotalEgress. Tiene umbrales diferentes para el ancho de banda en función del tipo de redundancia y regiones.

* [Objetivos de escalabilidad para blobs, colas, tablas y archivos](https://azure.microsoft.com/documentation/articles/storage-scalability-targets/#scalability-targets-for-blobs-queues-tables-and-files)

Compare TotalIngress y TotalEgress con los límites de entrada y salida de la región y el tipo de redundancia de la cuenta de almacenamiento.

Compruebe los límites de rendimiento de los discos duros virtuales asociados a la máquina virtual. Agregue las métricas de máquina virtual de lectura y escritura en el disco.

Cada disco duro virtual puede admitir hasta 60 MB/s (no se exponen las operaciones IOPS por disco duro virtual). Examine los datos para ver si va a alcanzar los límites de MB de rendimiento combinado de los discos duros virtuales en el nivel de máquina virtual mediante Lectura y escritura de disco y, luego, optimice la configuración del almacenamiento de máquina virtual para escalar los límites anteriores de un único disco duro virtual.

### <a name="high-disk-utilizationlatency-remediation"></a>Uso elevado del disco y corrección de la latencia

Reduzca la latencia del cliente y optimice la E/S de máquina virtual para escalar los límites anteriores del disco duro virtual.

* [Optimización de la E/S para Windows en Azure](https://azure.microsoft.com/documentation/articles/virtual-machines-sql-server-performance-best-practices/)

* [Optimización de la E/S para Linux en Azure](https://blogs.msdn.microsoft.com/igorpag/2014/10/23/azure-storage-secrets-and-linux-io-optimizations/)

#### <a name="reduce-throttling"></a>Reducción de la limitación

Si se alcanzan los límites superiores de las cuentas de almacenamiento, vuelva a equilibrar los discos duros virtuales entre ellas. Consulte [Objetivos de escalabilidad y rendimiento de Azure Storage](https://azure.microsoft.com/documentation/articles/storage-scalability-targets/).

### <a name="increase-throughput-and-reduce-latency"></a>Aumento del rendimiento y reducción de la latencia

Si tiene una aplicación que tiende a la latencia y requiere un alto rendimiento, migre los discos duros virtuales a Azure Premium Storage con la máquina virtual de la serie DS y GS.

En estos artículos se describen los escenarios específicos:

* [Migrar a Azure Premium Storage](https://azure.microsoft.com/documentation/articles/storage-migration-to-premium-storage/)

* [Uso de Azure Premium Storage con SQL Server](https://azure.microsoft.com/documentation/articles/virtual-machines-sql-server-use-premium-storage/)

## <a name="next-steps"></a>Pasos siguientes

Si necesita más ayuda con cualquier aspecto de este artículo, póngase en contacto con los expertos de Azure en los [foros de MSDN Azure o Stack Overflow](https://azure.microsoft.com/support/forums/).

Como alternativa, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.
