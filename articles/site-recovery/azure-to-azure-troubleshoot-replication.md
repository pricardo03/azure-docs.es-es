---
title: Solución de problemas y errores de la replicación de Azure a Azure en Azure Site Recovery| Microsoft Docs
description: Solución de problemas y errores al replicar máquinas virtuales de Azure para la recuperación ante desastres
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: troubleshooting
ms.date: 11/27/2018
ms.author: asgang
ms.openlocfilehash: 4a18e009f7defc8d41846b867f9b7a65d2b853dd
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/03/2019
ms.locfileid: "53993338"
---
# <a name="troubleshoot-azure-to-azure-vm-ongoing-replication-issues"></a>Solución de problemas de replicación de máquinas virtuales en curso de Azure a Azure

En este artículo se describen los problemas comunes de Azure Site Recovery al replicar y recuperar máquinas virtuales de Azure de una región a otra y se explica cómo solucionarlos. Para más información sobre las configuraciones admitidas, consulte la [matriz de compatibilidad para la replicación de máquinas virtuales de Azure](site-recovery-support-matrix-azure-to-azure.md).


## <a name="recovery-points-not-getting-generated"></a>No se están generando los puntos de recuperación

MENSAJE DE ERROR: No crash consistent recovery point available for the VM in the last 60 minutes (No se ha producido ningún punto de recuperación coherente con bloqueo de la máquina virtual en los últimos 60 minutos).</br>
ID. DEL ERROR: 153007 </br>

Azure Site Recovery replica los datos de manera coherente de la región de origen a la región de recuperación ante desastres y crea puntos coherentes por bloqueo cada 5 minutos. Si Site Recovery no puede crear puntos de recuperación durante 60 minutos, alerta de usuario. A continuación se muestran las posibles causas de este error:

**Causa 1: [velocidad de cambio elevada en los datos de la máquina virtual de origen](#high-data-change-rate-on-the-source-virtal-machine)**    
**Causa 2: [problema de conectividad de red](#Network-connectivity-issue)**

## <a name="causes-and-solutions"></a>Causas y soluciones

### <a name="high-data-change-rate-on-the-source-virtal-machine"></a>Velocidad de cambio elevada en los datos de la máquina virtual de origen
Azure Site Recovery desencadena un evento si la velocidad de cambio en la máquina virtual de origen supera los límites admitidos. Para comprobar si el problema se debe al alto grado de renovación, vaya a Elementos replicados > Máquina virtual > haga clic en "Events -last 72 hours" (Eventos: últimas 72 horas).
Debería ver el evento "Frecuencia de cambio de datos superior a los límites admitidos" como se muestra en la siguiente captura de pantalla

![data_change_rate_high](./media/site-recovery-azure-to-azure-troubleshoot/data_change_event.png)

Si hace clic en el evento, debería ver la información exacta del disco como se muestra en la siguiente captura de pantalla

![data_change_rate_event](./media/site-recovery-azure-to-azure-troubleshoot/data_change_event2.png)


#### <a name="azure-site-recovery-limits"></a>Límites de Azure Site Recovery
En la tabla siguiente se proporcionan los límites de Azure Site Recovery. Estos límites se basan en nuestras pruebas, pero no pueden cubrir todas las combinaciones de E/S posibles de la aplicación. Los resultados reales pueden variar en función de la combinación de E/S de la aplicación. También debemos mencionar que hay dos límites que tener en cuenta: renovación de datos por disco y por máquina virtual.
Por ejemplo, si miramos el disco Premium P20 de la siguiente tabla, Site Recovery puede controlar la renovación de 5 MB/s por disco con un máximo de cinco de estos discos por máquina virtual, debido al límite de renovación total de 25 MB/s por máquina virtual.

**Destino de almacenamiento de la replicación** | **Tamaño medio de E/S de disco de origen** |**Actividad de datos media de disco de origen** | **Actividad de datos de disco de origen total por día**
---|---|---|---
Standard Storage | 8 KB | 2 MB/s | 168 GB por disco
Disco Premium P10 o P15 | 8 KB  | 2 MB/s | 168 GB por disco
Disco Premium P10 o P15 | 16 KB | 4 MB/s |  336 GB por disco
Disco Premium P10 o P15 | 32 KB, o más | 8 MB/s | 672 GB por disco
Disco Premium P20, P30, P40 o P50 | 8 KB    | 5 MB/s | 421 GB por disco
Disco Premium P20, P30, P40 o P50 | 16 KB, o más |10 MB/s | 842 GB por disco

### <a name="solution"></a>Solución
Debemos ser conscientes de que Azure Site Recovery tiene límites de velocidad de cambio de datos en función del tipo de disco. Para saber si este problema es periódico o momentáneo, es importante buscar el patrón de velocidad de cambio en los datos en la máquina virtual afectada.
Para buscar la velocidad de cambio en los datos de la máquina virtual afectada. Vaya a la máquina virtual de origen > métricas de Supervisión y agregue las métricas como se muestra a continuación.

![high_data_change_rate](./media/site-recovery-azure-to-azure-troubleshoot/churn.png)

1. Haga clic en "Agregar métrica" y agregue "Bytes de escritura de discos del SO/s" y "Bytes de escritura de discos de datos/s".
2. Supervise el aumento como se muestra en la captura de pantalla.
3. Se mostrará una combinación de todas las operaciones de escritura que se producen en el disco del sistema operativo y en todos los discos de datos. Ahora estas métricas podrían no indicar por información de nivel de disco, pero son un buen indicador del patrón de renovación total de los datos.

En casos como el anterior, si es una ráfaga de datos ocasional y la velocidad de cambio de los datos es superior a 10 MBps (para Premium) y 2 MBps (para Estándar) durante algún tiempo y desciende, la replicación mantendrá el ritmo. Sin embargo, si la renovación supera con creces el límite admitido la mayoría del tiempo, debería considerar una de las siguientes opciones (si es posible):

**Opción 1:** excluya el disco que provoca la alta velocidad de cambio en los datos: </br>
Actualmente puede excluir el disco con [Powershell de Site Recovery](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-powershell#replicate-azure-virtual-machine).

**Opción 2:** cambie el nivel de disco de almacenamiento de recuperación ante desastres: </br>
Esta opción solo es posible si la renovación de datos de disco tiene una velocidad inferior a 10 MB/s. Digamos que una máquina virtual con un disco P10 renueva datos a una velocidad mayor que 8 MB/s pero menor que 10 MB/s. Si el cliente puede usar un disco P30 de almacenamiento de destino durante la protección, el problema se puede resolver.

### <a name="Network-connectivity-issue"></a>Problema de conectividad de red

#### <a name="network-latency-to-cache-storage-account-"></a>Latencia de red en la cuenta de almacenamiento en caché:
 Site Recovery envía datos replicados a la cuenta de almacenamiento en caché. Este envío puede producirse si la velocidad de carga desde la máquina virtual hasta la cuenta de almacenamiento en caché es inferior a 4 MB cada 3 segundos. Para comprobar si hay algún problema relacionado con la latencia, utilice [azcopy](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy) para cargar datos desde la máquina virtual en la cuenta de almacenamiento en caché.<br>
Si la latencia es alta, compruebe si está utilizando aplicaciones virtuales de red para controlar el tráfico que sale de las máquinas virtuales. Es posible que se genere un cuello de botella si todo el tráfico de replicación pasa por la aplicación virtual de red. Se recomienda crear un punto de conexión de servicio de red en la red virtual de "Storage" para que el tráfico de replicación no se dirija a la NVA. Consulte [Configuración de la aplicación virtual de red](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#network-virtual-appliance-configuration).

#### <a name="network-connectivity"></a>Conectividad de red
Para que la replicación de Site Recovery funcione, la máquina virtual debe disponer de conectividad saliente a direcciones URL o intervalos IP específicos. Si la máquina virtual está detrás de un firewall o usa reglas de grupo de seguridad de red (NSG) para controlar la conectividad saliente, puede encontrarse alguno de estos problemas.</br>
Consulte [Conectividad de salida para las direcciones URL de Site Recovery](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges) para comprobar que todas las direcciones URL están conectadas. 