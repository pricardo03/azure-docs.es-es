---
title: Configuraciones y operaciones de infraestructura de SAP HANA en Azure | Microsoft Docs
description: Guía de operaciones para los sistemas SAP HANA que se implementan en Azure Virtual Machines.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/04/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: acccc553c5b63b2acd0f9793b0397b25145449dd
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59699345"
---
# <a name="sap-hana-infrastructure-configurations-and-operations-on-azure"></a>Configuraciones y operaciones de infraestructura de SAP HANA en Azure
Este artículo proporcionan instrucciones sobre cómo configurar la infraestructura de Azure y operar los sistemas de SAP HANA que se implementan en nativas máquinas virtuales (VM). En este artículo también incluye información de configuración de SAP HANA de escalado horizontal de la SKU de máquina virtual M128s. En este artículo no pretende reemplazar la documentación de SAP estándar, que incluye el siguiente contenido:

- [Guía de administración de SAP](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [Guías de instalación de SAP](https://service.sap.com/instguides)
- [Notas de SAP](https://sservice.sap.com/notes)

## <a name="prerequisites"></a>Requisitos previos
Para utilizar esta guía, necesita un conocimiento básico de los siguientes componentes de Azure:

- [Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Redes de azure y Azure Virtual Network](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure Storage](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Para obtener más información acerca de SAP NetWeaver y otros componentes de SAP en Azure, consulte la sección [SAP en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) de la [documentación de Azure](https://docs.microsoft.com/azure/).

## <a name="basic-setup-considerations"></a>Consideraciones básicas sobre la configuración
En las siguientes secciones se describen las consideraciones de configuración básicas para implementar sistemas SAP HANA en máquinas virtuales de Azure.

### <a name="connect-to-azure-virtual-machines"></a>Conectarse a máquinas virtuales de Azure
Como se documenta en el [máquinas virtuales de Azure Guía de planificación](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide), se usan dos métodos básicos para conectarse a máquinas virtuales de Azure:

- Conectarse a través de internet y puntos de conexión públicos en una VM de JumpBox o en la máquina virtual que ejecuta SAP HANA.
- Conéctese a través de una [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) o de Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

La conectividad de sitio a sitio a través de VPN o ExpressRoute es necesaria para los escenarios de producción. Este tipo de conexión también es necesario para escenarios de producción que se incorporan en escenarios de producción donde se usa el software de SAP. La imagen siguiente muestra un ejemplo de conectividad entre sitios:

![Conectividad entre sitios](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choose-azure-vm-types"></a>Elegir tipos de máquina virtual de Azure
Los tipos de máquina virtual de Azure que se usará para escenarios de producción se muestran en el [documentación de SAP para IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). Para escenarios de producción, una mayor variedad de tipos nativos de máquina virtual de Azure está disponible.

>[!NOTE]
> Para escenarios de producción, use los tipos de máquina virtual que se muestran en el [SAP nota 1928533](https://launchpad.support.sap.com/#/notes/1928533). Para el uso de máquinas virtuales de Azure para escenarios de producción, compruebe las máquinas virtuales de SAP publicada certificadas para SAP HANA [certified lista de plataformas de IaaS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure).

Para implementar las máquinas virtuales de Azure, use:

- Azure Portal.
- Los cmdlets de Azure PowerShell.
- La CLI de Azure.

También puede implementar una plataforma SAP HANA completamente instalada en los servicios de máquina virtual de Azure a través de la [plataforma en la nube SAP](https://cal.sap.com/). Para obtener información sobre el proceso de instalación, consulte [implementación de SAP S/4HANA o BW/4HANA en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h). Para obtener información sobre la automatización publicada, consulte [en este artículo de GitHub](https://github.com/AzureCAT-GSI/SAP-HANA-ARM).

### <a name="choose-an-azure-storage-type"></a>Elija un tipo de almacenamiento de Azure
Azure proporciona dos tipos de almacenamiento que son adecuados para máquinas virtuales de Azure que ejecutan SAP HANA:  

- Unidades de disco duro (HDD) estándares
- Unidades de estado sólido (SSD) Premium

Para obtener información acerca de estos tipos de disco, consulte [seleccione un tipo de disco](../../windows/disks-types.md).

Azure ofrece dos métodos de implementación para los discos duros virtuales en Azure storage estándar y premium storage. Si el escenario global lo permite, aproveche las implementaciones de [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/).

Para obtener una lista de tipos de almacenamiento y sus SLA de IOPS y rendimiento del almacenamiento, consulte [documentación de Azure para managed disks](https://azure.microsoft.com/pricing/details/managed-disks/).

### <a name="configure-the-storage-for-azure-virtual-machines"></a>Configurar el almacenamiento para máquinas virtuales de Azure

Probablemente nunca importaba sobre sus capacidades y los subsistemas de E/S porque el proveedor del dispositivo se aseguró de que se cumplen los requisitos de almacenamiento mínimo para SAP HANA. Cuando se compila la infraestructura de Azure, deberá tener en cuenta algunos de esos requisitos. También debe comprender los requisitos de configuración sugeridos en las secciones siguientes. Para los casos donde configura las máquinas virtuales que desea que SAP HANA que se ejecutan en, es posible que necesite:

- Habilitar el volumen de lectura/escritura en/Hana/log como mínimo de 250 MB/s para tamaños de E/S de 1 MB.
- Habilitar la actividad de lectura de al menos 400 MB/s para/Hana/Data para los tamaños de E/S de 16 MB y 64 MB.
- Habilitar la actividad de escritura de al menos 250 MB/s para/Hana/Data con tamaños de E/S de 16 MB y 64 MB.

Almacenamiento de baja latencia es muy importante para sistemas DBMS, incluso si el DBMS, como SAP HANA, mantiene los datos en memoria. La ruta crítica en el almacenamiento normalmente está relacionada con las escrituras del registro de transacciones de los sistemas DBMS. Pero las operaciones, como escribir puntos de retorno o cargar datos en memoria después de recuperación tras bloqueo también puede ser crítico. 

Es obligatorio usar discos premium de Azure para los volúmenes/Hana/Data y/Hana/log. Para lograr el rendimiento mínimo de/Hana/log y/Hana/Data como desea SAP, compilar un RAID 0 utilizando mdadm o un administrador de volúmenes lógicos (LVM) en varios discos de Azure premium storage. También usar los volúmenes de RAID como/Hana/Data y volúmenes/Hana/log. Se recomienda que use los siguientes tamaños de franja para RAID 0:

- 64 KB o 128 KB para/hana/data
- 32 KB para/hana/log

> [!NOTE]
> No es necesario configurar ningún nivel de redundancia con volúmenes RAID ya que Azure premium y standard storage mantienen tres imágenes de un disco duro virtual. El uso de un volumen RAID es puramente configurar volúmenes que proporcionen el rendimiento de E/S suficiente.

Acumulación de un número de discos duros virtuales de Azure bajo un RAID es acumulativo por parte de un lado de rendimiento IOPS y el almacenamiento. Si coloca una RAID 0 con 3 discos de almacenamiento de Azure premium P30, le proporciona tres veces la IOPS y tres veces el rendimiento de almacenamiento de un disco P30 de almacenamiento premium de Azure único.

Las siguientes recomendaciones de almacenamiento en caché, supongamos que las características de E/S para SAP HANA:

- No hay casi cualquier carga de trabajo de lectura frente a los archivos de datos. Las excepciones son las operaciones de E/s de gran tamaño después del reinicio de la instancia de HANA o cuando se cargan datos en HANA. Otro caso de operaciones de E/S de lectura de mayor tamaño con archivos de datos pueden ser las copias de seguridad de base de datos de HANA. Como resultado, la memoria caché de lectura no tiene sentido porque, en la mayoría de los casos, se deben leer completamente todos los volúmenes de archivo de datos.
- La escritura en los archivos de datos se produce en ráfagas según los puntos de retorno de HANA y la recuperación tras bloqueos de HANA. Escribir puntos de retorno es asincrónico y no da la talla ninguna transacción de usuario. La escritura de datos durante la recuperación tras bloqueo es esencial en el rendimiento a fin de lograr que el sistema responda de nuevo rápidamente. Recuperación tras bloqueo debe ser situaciones excepcionales en su lugar.
- Apenas hay lecturas de los archivos de puesta al día de HANA. Las excepciones son las operaciones de E/s grandes al realizar copias de seguridad de registro de transacciones, recuperación tras bloqueo o la fase de reinicio de una instancia de HANA.  
- La carga principal con el archivo de registro de puesta al día de SAP HANA es escrituras. Según la naturaleza de la carga de trabajo, puede tener las E/s tan pequeña como 4 KB o, en otros casos, las operaciones de E/s el tamaño de 1 MB o más. La latencia de las escrituras en relación con el registro de puesta al día SAP HANA es esencial para el rendimiento.
- Todas las escrituras deben conservarse en el disco de forma confiable.

Como resultado de estos patrones de E/S observados por SAP HANA, establezca el almacenamiento en caché de los diferentes volúmenes que usan Azure premium storage para:

- **/hana/data**: Sin almacenamiento en caché.
- **/hana/log**: Sin almacenamiento en caché, con una excepción para máquinas virtuales de la serie M (vea más adelante en este artículo).
- **/hana/shared**: Almacenamiento en caché de lectura.


Además, tenga en cuenta el rendimiento global de E/S de la máquina virtual al cambiar el tamaño o decidir en una máquina virtual. En general el rendimiento del almacenamiento de máquina virtual se documenta en [tamaños de máquina virtual optimizada para memoria](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory).

#### <a name="linux-io-scheduler-mode"></a>Modo de programador de E/S de Linux
Linux tiene varios modos diferentes de programación de E/S. Una recomendación habitual de SAP y los proveedores de Linux consiste en establecer el modo de programador de E/S para los volúmenes de disco fuera de la **cfq** modo a la **noop** modo. Para obtener más información, consulte [SAP nota #1984798](https://launchpad.support.sap.com/#/notes/1984787). 


#### <a name="storage-solution-with-write-accelerator-for-azure-m-series-virtual-machines"></a>Solución de almacenamiento con acelerador de escritura para Azure las máquinas virtuales de la serie M
 Acelerador de escritura que es una característica de Azure que está implementando en Azure Virtual Machines de la serie M exclusivamente. El propósito de la funcionalidad es mejorar la latencia de E/S de escrituras en Azure premium storage. Para SAP HANA, el Acelerador de escritura se supone que debe usarse solo en el volumen /hana/log. Por este motivo, deben cambiar las configuraciones mostradas hasta ahora. El cambio principal es el desglose entre/Hana/Data y/Hana/log para poder usar el Acelerador de escritura en el volumen/Hana/log solo. 

> [!IMPORTANT]
> Certificación de SAP HANA en Azure serie M de las máquinas virtuales es exclusivo de Acelerador de escritura para el volumen/hana/log. Como resultado, las implementaciones de SAP HANA de escenario de producción en Azure serie M de las máquinas virtuales deben configurarse con acelerador de escritura para/hana/volumen del registro.

> [!NOTE]
> Para escenarios de producción, compruebe si un determinado tipo de máquina virtual es compatible con SAP HANA de SAP en la [documentación de SAP para IaaS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

La siguiente tabla muestra las configuraciones recomendadas.

| SKU de la máquina virtual | RAM | E/S de máximo de VM<br /> throughput | /hana/data | /hana/log | /hana/shared | /root volume | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| M32ts | 192 GiB | 500 MB/seg. | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M32ls | 256 GiB | 500 MB/seg. | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M64ls | 512 GB | 1000 MB/seg. | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P30 |
| M64s | 1000 giB | 1000 MB/seg. | 4 x P20 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1750 giB | 1000 MB/seg. | 3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128s | 2.000 giB | 2000 MB/seg. |3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P40 |
| M128ms | 3.800 giB | 2000 MB/seg. | 5 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P50 |

Compruebe si el rendimiento del almacenamiento para los diferentes volúmenes sugeridos cumple con la carga de trabajo que desea ejecutar. Si la carga de trabajo requiere volúmenes mayores para/Hana/Data y/Hana/log, aumentar el número de discos duros virtuales de almacenamiento de Azure premium. Ajustar el tamaño de un volumen con más discos duros virtuales que enumerados aumenta el número de IOPS y rendimiento de E/S dentro de los límites del tipo de máquina virtual de Azure.

El Acelerador de escritura solo funciona en conjunto con [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/). Como mínimo, los discos de almacenamiento premium de Azure que forman el volumen/Hana/log deben implementarse como discos administrados.

Existen límites para el almacenamiento premium de Azure VHD por máquina virtual que puede admitir el Acelerador de escritura. Los límites actuales son:

- 16 VHD para una M128xx máquina virtual.
- 8 VHD para una M64xx máquina virtual.
- 4 discos duros virtuales para una M32xx máquina virtual.

Para obtener más información sobre cómo habilitar el Acelerador de escritura, consulte [Acelerador de escritura](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator).

Para obtener más información sobre y restricciones para el Acelerador de escritura, consulte la documentación del misma.


#### <a name="cost-conscious-azure-storage-configuration"></a>Configuración de costos de almacenamiento de Azure
En la tabla siguiente se muestra una configuración de tipos de VM que los clientes suelen usar para hospedar SAP HANA en máquinas virtuales de Azure. Puede haber algunos tipos de máquinas virtuales que no cumplen todos los criterios mínimos para SAP HANA. También podría haber algunos tipos de máquinas virtuales que no son oficialmente compatibles con SAP HANA con SAP. Hasta ahora, esas máquinas virtuales se han realizado correctamente para escenarios de producción. 

> [!NOTE]
> Para escenarios de producción, compruebe si un determinado tipo de máquina virtual es compatible con SAP HANA de SAP en la [documentación de SAP para IaaS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).


| SKU de la máquina virtual | RAM | E/S de máximo de VM<br /> throughput | /hana/data y /hana/log<br /> seccionado con LVM o mdaadm | /hana/shared | /root volume | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 112 GiB | 768 MB/seg. | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E16v3 | 128 GB | 384 MB/seg. | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E32v3 | 256 GiB | 768 MB/seg. | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| E64v3 | 432 GiB | 1200 MB/seg. | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| GS5 | 448 GiB | 2000 MB/seg. | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| M32ts | 192 GiB | 500 MB/seg. | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M32ls | 256 GiB | 500 MB/seg. | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M64ls | 512 GB | 1000 MB/seg. | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 |1 x S30 |
| M64s | 1000 giB | 1000 MB/seg. | 2 x P30 | 1 x S30 | 1 x S6 | 1 x S6 |2 x S30 |
| M64ms | 1750 giB | 1000 MB/seg. | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128s | 2.000 giB | 2000 MB/seg. |3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S40 |
| M128ms | 3.800 giB | 2000 MB/seg. | 5 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S50 |


Los discos que se recomiendan para los tipos de la máquina virtual más pequeña con exceso de 3 x P20 los volúmenes con respecto a las recomendaciones de espacio que se proporcionan en el [notas de SAP TDI almacenamiento](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). La opción que se muestra en la tabla se realizó para proporcionar el rendimiento de disco suficiente para SAP HANA. Si necesita cambiar la **/hana/backup** volumen, que se estableció un tamaño para mantener copias de seguridad que representan dos veces el volumen de memoria, no dude en realizar ajustes. 

Compruebe si el rendimiento del almacenamiento para los diferentes volúmenes sugeridos cumple con la carga de trabajo que desea ejecutar. Si la carga de trabajo requiere volúmenes mayores para/Hana/Data y/Hana/log, aumentar el número de discos duros virtuales de almacenamiento de Azure premium. Ajustar el tamaño de un volumen con más discos duros virtuales que enumerados aumenta el número de IOPS y rendimiento de E/S dentro de los límites del tipo de máquina virtual de Azure. 

> [!NOTE]
> No se benefician de las configuraciones anteriores [único de máquina virtual de Azure VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) porque usa una mezcla de almacenamiento premium de Azure y Azure standard storage. La selección se ha elegido para optimizar los costos. Elija premium storage para todos los discos de la tabla que se muestran como Azure standard storage (Sxx) para que la configuración de máquina virtual compatible con el SLA de máquina virtual de Azure único.


> [!NOTE]
> Las recomendaciones de configuración de disco de destino los requisitos mínimos que SAP proporciona a los proveedores de su infraestructura. En las implementaciones de clientes reales y escenarios de carga de trabajo, estas recomendaciones no proporcionan capacidades suficientes en algunas situaciones. Por ejemplo, un cliente necesitó una recarga de los datos más rápida después de un reinicio HANA o configuraciones de copia de seguridad requiere mayor ancho de banda para el almacenamiento. Otros casos incluyen/Hana/log, donde 5000 e/s por segundo no eran suficientes para la carga de trabajo específica. Siga estas recomendaciones como punto de partida y adáptelos según los requisitos de la carga de trabajo.
>  

### <a name="set-up-azure-virtual-networks"></a>Configurar Azure Virtual Network
Cuando haya conectividad de sitio a sitio a Azure a través de VPN o ExpressRoute de Azure, debe tener al menos una red virtual de Azure que está conectada a través de una puerta de enlace virtual al circuito VPN o ExpressRoute. En las implementaciones sencillas, la puerta de enlace virtual puede implementarse en una subred de la red virtual de Azure que hospeda las instancias de SAP HANA demasiado. 

Para instalar SAP HANA, cree dos subredes adicionales en la red virtual de Azure. Una subred hospeda las máquinas virtuales para ejecutar las instancias de SAP HANA. La otra subred ejecuta JumpBox o máquinas virtuales de administración para hospedar SAP HANA Studio, otro software de administración o el software de aplicación.

> [!IMPORTANT]
> Configurar [aplicaciones virtuales de red de Azure](https://azure.microsoft.com/solutions/network-appliances/) en la ruta de comunicación entre la aplicación de SAP y la capa de DBMS de un SAP NetWeaver-, Hybris o del sistema basado en S/4HANA SAP no se admite. Esta restricción es por motivos de rendimiento y funcionalidad. La ruta de comunicación entre la capa de aplicación de SAP y la capa de DBMS debe ser un directa. No incluye la restricción [reglas (NSG) de grupo de seguridad de red y grupo de seguridad de aplicaciones (ASG)](https://docs.microsoft.com/azure/virtual-network/security-overview) si esas reglas ASG y NSG permiten una ruta de acceso de comunicación directa. 
>
> Otros escenarios donde no se admiten las aplicaciones virtuales de red se encuentran en: 
>
> - Rutas de comunicación entre máquinas virtuales de Azure que representan Linux Pacemaker clúster nodos y dispositivos SBD como se describe en [alta disponibilidad para SAP NetWeaver en máquinas virtuales de Azure en SUSE Linux Enterprise Server para SAP Applications](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse).
> - Rutas de comunicación entre máquinas virtuales de Azure y Windows Server Scale-Out File Server como se describe en establecer [agrupar una instancia ASCS/SCS de SAP en un clúster de conmutación por error de Windows mediante el uso de un recurso compartido de archivos en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share). 
>
> Aplicaciones virtuales de red en las rutas de comunicación pueden duplicar fácilmente la latencia de red entre los asociados de comunicación de dos. También puede restringir el rendimiento en las rutas críticas entre la capa de aplicación de SAP y la capa de DBMS. En algunos escenarios de clientes, aplicaciones virtuales de red pueden provocar un error en los clústeres de Linux Pacemaker. Estos son los casos donde las comunicaciones entre los nodos del clúster de Linux Pacemaker comunican con su dispositivo SBD a través de una aplicación virtual de red.  
> 

> [!IMPORTANT]
> Otro diseño del *no* admite es la segregación de la capa de aplicación de SAP y la capa DBMS en diferentes redes virtuales de Azure que no son [emparejadas](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) entre sí. Se recomienda separar las capas de aplicación de SAP y DBMS mediante subredes dentro de una red virtual de Azure en lugar de a través de diferentes redes virtuales de Azure. 
>
>Si decide no sigue la recomendación y en su lugar, separar las dos capas en diferentes redes virtuales, las dos redes virtuales deben ser [emparejadas](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview). 
>
> Tenga en cuenta que el tráfico entre dos de red [emparejadas](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) redes virtuales de Azure está sujeto a los costes de transferencia. Volumen de datos enorme que consta de muchos terabytes se intercambia entre las capas de aplicación de SAP y DBMS. Puede acumular los costos sustanciales si las capas de aplicación de SAP y DBMS se segregan entre dos redes virtuales emparejadas Azure. 

Al instalar las máquinas virtuales para ejecutar SAP HANA, estas necesitan lo siguiente:

- Dos NIC virtuales instaladas. Una NIC se conecta a la subred de administración. Otra NIC se conecta desde la red local u otras redes a la instancia de SAP HANA en la máquina virtual de Azure.
- Direcciones IP privadas estáticas implementadas para ambas NIC virtuales.

> [!NOTE]
> Asignar direcciones IP estáticas a través de Azure significa que los asigne a cada NIC virtuales. No asignar direcciones IP estáticas en el sistema operativo invitado a una NIC virtual. Algunos servicios de Azure como copia de seguridad de Azure se basan en el hecho de que al menos la NIC virtual principal se establece en DHCP y no a las direcciones IP estáticas. Para obtener más información, consulte [copia de seguridad de máquina virtual de solución de problemas de Azure](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking). Para asignar varias direcciones IP estáticas a una máquina virtual, asigne varias NIC virtuales a una máquina virtual.
>
>

Para las implementaciones que son permanentes, cree una arquitectura de red del centro de datos virtual en Azure. Esta arquitectura, recomienda la separación de la puerta de enlace de red virtual de Azure que se conecta a un entorno local en una red virtual de Azure independiente. Esta red virtual independiente hospeda todo el tráfico que sale en el entorno local o a internet. Con este enfoque, puede implementar software para auditoría y registro del tráfico que entra en el centro de datos virtual de Azure en esta red virtual de concentrador independiente. De este modo, tener una red virtual que hospeda todo el software y las configuraciones que se relaciona con tráfico de entrada y salido para la implementación de Azure.


Para obtener más información sobre el enfoque de centro de datos virtual y el diseño de red virtual de Azure relacionados, consulte: 

- [Centro de datos virtual de Azure: Una perspectiva de red](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter).
- [Centro de datos virtual de Azure y el plano de control empresarial](https://docs.microsoft.com/azure/architecture/vdc/).


>[!NOTE]
>El tráfico que fluye entre una red virtual de concentrador y una red virtual de radios mediante el uso de [emparejamiento de red virtual de Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) está sujeta a adicionales [costos](https://azure.microsoft.com/pricing/details/virtual-network/). Según esos costos, es posible que deba realizar compromisos entre la ejecución de un diseño de red de concentrador y radio estricta y varios [puertas de enlace de Azure ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways) conectarse a los radios con el fin de omitir el emparejamiento de redes virtuales. 
>
> Las puertas de enlace de Azure ExpressRoute introducen adicionales [costos](https://azure.microsoft.com/pricing/details/vpn-gateway/) demasiado. También se podrían producir costos adicionales de software de terceros que usar para iniciar sesión, auditar y supervisar el tráfico de red. Tenga en cuenta los costos para el intercambio de datos a través de emparejamiento frente a los costos creados por otras puertas de enlace de ExpressRoute y las licencias de software de red virtual. Podría decidir en microsegmentar una red virtual mediante el uso de las subredes como unidades de aislamiento en lugar de las redes virtuales.


Para obtener información general de los distintos métodos que puede utilizar para asignar direcciones IP, consulte [tipos y métodos de asignación de Azure de direcciones IP](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

Para máquinas virtuales que ejecutan SAP HANA, trabajar con las direcciones IP estáticas que están asignadas. La razón es que algunos atributos de configuración de HANA hacen referencia a las direcciones IP.

[NSG de Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) se usan para dirigir el tráfico se enruta a la instancia de SAP HANA o JumpBox. Los NSG y, finalmente, [grupos de seguridad de la aplicación](https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups) están asociados con la subred de SAP HANA y la administración.

La siguiente imagen muestra una visión general de un esquema de implementación aproximado para SAP HANA que sigue a una arquitectura de red virtual de concentrador y radio:

![Esquema de implementación aproximado de SAP HANA](media/hana-vm-operations/hana-simple-networking.PNG)

Para implementar SAP HANA en Azure sin una conexión de sitio a sitio, proteger la instancia de SAP HANA desde la red internet pública y se oculta detrás de un proxy de reenvío. En este escenario básico, la implementación se basa en Azure servicios DNS integrados para resolver los nombres de host. En una implementación más compleja en la que se usan direcciones IP de acceso público, los servicios DNS integrados en Azure son especialmente importantes. Usar los NSG de Azure y [aplicaciones virtuales de red de Azure](https://azure.microsoft.com/solutions/network-appliances/) para supervisar el enrutamiento de internet en la arquitectura de red virtual de Azure en Azure. 

La siguiente imagen muestra un esquema aproximado de cómo implementar SAP HANA sin una conexión de sitio a sitio en una arquitectura de red virtual de concentrador y radio:
  
![Esquema de implementación aproximado de SAP HANA sin una conexión de sitio a sitio](media/hana-vm-operations/hana-simple-networking2.PNG)
 

Para obtener otra descripción de cómo usar aplicaciones virtuales de red de Azure para controlar y supervisar el acceso desde internet sin la arquitectura de red virtual de concentrador y radio, consulte [implementar aplicaciones virtuales de red de alta disponibilidad](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha).


## <a name="configure-azure-infrastructure-for-sap-hana-scale-out"></a>Configurar la infraestructura de Azure para SAP HANA de escalado horizontal
Microsoft tiene una serie M VM SKU que está certificado para una configuración de escalado horizontal de SAP HANA. El tipo de máquina virtual M128s está certificado para una implementación escalada de hasta 16 nodos. Para realizar cambios en las certificaciones de escalabilidad horizontal de SAP HANA en máquinas virtuales de Azure, consulte el [certified lista de plataformas de IaaS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure).

Las versiones del sistema operativo mínimas utilizadas para implementar configuraciones de escalado horizontal en Azure Virtual Machines son:

- SUSE Linux 12 SP3.
- Red Hat Linux 7.4.

Para la certificación de escalabilidad horizontal de 16 nodos:

- Un nodo es el nodo maestro.
- Un máximo de 15 nodos son nodos de trabajo.

>[!NOTE]
>En las implementaciones de escalado de máquina virtual de Azure, no es posible usar un nodo en espera.
>

Hay dos razones por qué no se puede configurar un nodo en espera:

- En este momento, Azure no tiene un servicio NFS nativo. Como resultado, se deben configurar recursos compartidos de NFS con la Ayuda de la funcionalidad de terceros.
- Ninguna de las configuraciones de NFS de terceros pueden cumplir los criterios de latencia de almacenamiento para SAP HANA con sus soluciones implementadas en Azure.

Como resultado, no se pueden compartir los volúmenes/Hana/Data y/Hana/log. No se comparte estos volúmenes de los nodos solo impide el uso de un nodo en espera de SAP HANA en una configuración de escalado horizontal.

La siguiente imagen muestra el diseño básico de un solo nodo en una configuración de escalado horizontal:

![Conceptos básicos de escalabilidad horizontal de un solo nodo](media/hana-vm-operations/scale-out-basics.PNG)

La configuración básica de un nodo de máquina virtual para escalabilidad horizontal de SAP HANA presenta este aspecto:

- Para/Hana/Shared, creará un clúster NFS de alta disponibilidad basado en SUSE Linux 12 SP3. Este clúster hospeda los recursos compartidos NFS / Hana/Shared de la configuración de escalado horizontal y SAP NetWeaver o BW/4HANA los servicios centrales. Para obtener información sobre cómo crear este tipo de configuración, consulte [alta disponibilidad para NFS en máquinas virtuales de Azure en SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs).
- Todos los demás volúmenes de disco no se comparten entre los nodos de diferentes y no se basan en NFS. Más adelante en este artículo se proporcionan configuraciones de instalación y pasos para las instalaciones de HANA de escalado horizontal con no compartido/Hana/Data y/Hana/log.

>[!NOTE]
>El clúster NFS de alta disponibilidad, como se muestra en los gráficos, hasta ahora, solo se admite con SUSE Linux. Más adelante se aconsejará una solución NFS de alta disponibilidad basada en Red Hat.

Ajustar el tamaño de los volúmenes para los nodos es el mismo que para el escalado vertical, excepto para/Hana/Shared. En la tabla siguiente se muestra los tipos y tamaños sugeridos para la SKU de máquina virtual M128s.

| SKU de la máquina virtual | RAM | E/S de máximo de VM<br /> throughput | /hana/data | /hana/log | /root volume | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | --- |
| M128s | 2.000 giB | 2000 MB/s |3 x P30 | 2 x P20 | 1 x P6 | 1 x P6 | 2 x P40 |


Compruebe si el rendimiento del almacenamiento para los diferentes volúmenes sugeridos cumple con la carga de trabajo que desea ejecutar. Si la carga de trabajo requiere volúmenes mayores para/Hana/Data y/Hana/log, aumentar el número de discos duros virtuales de almacenamiento de Azure premium. Ajustar el tamaño de un volumen con más discos duros virtuales que enumerados aumenta el número de IOPS y rendimiento de E/S dentro de los límites del tipo de máquina virtual de Azure. Acelerador de escritura también se aplican a los discos que forman el volumen/Hana/log.
 

Para una fórmula que define el tamaño del volumen/hana/shared de escalado horizontal como el tamaño de memoria de un único nodo de trabajo por cuatro nodos de trabajo, consulte [los requisitos de almacenamiento de SAP HANA TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html).

Suponiendo que aprovechar la VM de Azure de M128s certificadas de SAP HANA de escalado horizontal con aproximadamente 2 TB de memoria, se resumen las recomendaciones de SAP como:

- Para un nodo maestro y hasta cuatro nodos de trabajo, el tamaño del volumen/Hana/Shared es de 2 TB.
- Para un nodo maestro y cinco y ocho nodos de trabajo, el tamaño del volumen/Hana/Shared es de 4 TB.
- Para un nodo maestro y 9 y 12 nodos de trabajo, el tamaño del volumen/Hana/Shared es 6 TB.
- Para un nodo maestro y nodos de trabajo de 12 a 15, el tamaño del volumen/Hana/Shared es 8 TB.

El diseño importantes que se muestra en los gráficos de la configuración de nodo único para una VM de SAP HANA de escalado horizontal es la red virtual con la configuración de subred. SAP recomienda una separación del tráfico de cara al cliente y aplicación de las comunicaciones entre los nodos HANA. 

Para lograr este objetivo, adjuntar dos NIC virtuales diferentes a la máquina virtual, como se muestra en los gráficos. Ambas NIC virtuales está en subredes diferentes y tiene dos direcciones IP diferentes. A continuación, controlar el flujo de tráfico con reglas de enrutamiento mediante NSG o las rutas definidas por el usuario.

Especialmente en Azure, no hay ningún medio y métodos para exigir calidad de servicio y las cuotas en NIC virtuales específicas. Como resultado, la separación de la comunicación entre nodos y de cara al cliente y aplicaciones no abre cualquier oportunidad de dar prioridad a un flujo de tráfico a través de la otra. En su lugar, la separación sigue siendo una medida de seguridad en las comunicaciones entre nodos de las configuraciones de escalado horizontal de blindaje.  

>[!IMPORTANT]
>SAP recomienda encarecidamente que separar el tráfico de red para el cliente y la aplicación dentro de un nodo y tráfico como se describe en este artículo. Le recomendamos que coloque una arquitectura en su lugar como se muestra en los gráficos anteriores.
>

La siguiente imagen muestra la arquitectura de red mínimos necesarios desde un punto de vista redes:

![Conceptos básicos de escalabilidad horizontal de un solo nodo](media/hana-vm-operations/scale-out-networking-overview.PNG)

Los límites que admite hasta ahora son 15 nodos de trabajo, además de un nodo maestro.

La siguiente imagen muestra la arquitectura de almacenamiento desde un punto de vista de almacenamiento:


![Conceptos básicos de escalabilidad horizontal de un solo nodo](media/hana-vm-operations/scale-out-storage-overview.PNG)

El volumen/Hana/Shared se encuentra en la configuración de recurso compartido NFS altamente disponible. Todas las unidades se montan localmente en las máquinas virtuales individuales. 

### <a name="highly-available-nfs-share"></a>Recurso compartido NFS de alta disponibilidad
Hasta ahora, el clúster NFS de alta disponibilidad está trabajando con SUSE Linux solo. Para obtener información del programa de instalación, consulte [alta disponibilidad para NFS en máquinas virtuales de Azure en SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs). Si no comparte el clúster NFS con cualquier otra configuración de HANA fuera de la red virtual de Azure que ejecuta las instancias de SAP HANA, instálelo en la misma red virtual. Instálelo en su propia subred y asegúrese de que no todo el tráfico arbitrario puede tener acceso a la subred. En su lugar, limite el tráfico a esa subred para las direcciones IP de la máquina virtual que se ejecutan en el tráfico de volumen/Hana/Shared.

Relacionado con la NIC virtual de una máquina virtual de escalabilidad horizontal HANA que enruta el tráfico / Hana/Shared, las recomendaciones son:

- Dado que el tráfico a/Hana/Shared es moderado, enrutar a través de la NIC virtual que se asigna a la red de cliente en la configuración mínima.
- Finalmente, para el tráfico a/hana/shared, implemente una tercera subred en la red virtual en el que implementar la configuración de escalado horizontal de SAP HANA. Asignar una NIC virtual de terceros que se hospeda en esa subred. Utilice la tercera tarjeta NIC virtual y la dirección IP asociada para el tráfico del recurso compartido NFS. Después puede aplicar reglas de enrutamiento y de acceso independientes.

>[!IMPORTANT]
>Tráfico de red entre las máquinas virtuales con SAP HANA de una manera de escalabilidad horizontal implementada y la alta disponibilidad NFS bajo ninguna circunstancia puede enrutarse a través de un [aplicación virtual de red](https://azure.microsoft.com/solutions/network-appliances/) o dispositivos virtuales similares. NSG de Azure no son estos dispositivos. Compruebe las reglas de enrutamientos para asegurarse de que desvió de aplicaciones virtuales de red o dispositivos virtuales similares cuando acceden a la alta disponibilidad NFS compartan desde las máquinas virtuales que ejecutan SAP HANA.
> 

Si desea compartir el clúster NFS de alta disponibilidad entre las configuraciones de SAP HANA, mueva todas esas configuraciones de HANA en la misma red virtual. 
 

### <a name="install-an-sap-hana-scale-out-in-azure"></a>Instalar un escalado horizontal de SAP HANA en Azure
Para instalar una configuración de escalado horizontal SAP, siga estos pasos generales:

- Implementar de nuevo o adaptar la nueva infraestructura de red virtual de Azure.
- Implemente las nuevas máquinas virtuales mediante el uso de volúmenes de almacenamiento premium de Azure administrado.
- Implementar una nueva o adaptar un clúster NFS de alta disponibilidad existente.
- Adaptar el enrutamiento de red para asegurarse de que, por ejemplo, la comunicación entre nodos entre las máquinas virtuales no se enruta a través de un [aplicación virtual de red](https://azure.microsoft.com/solutions/network-appliances/). Lo mismo puede decirse para el tráfico entre las máquinas virtuales y el clúster NFS de alta disponibilidad.
- Instalar el nodo maestro de SAP HANA.
- Adapte los parámetros de configuración del nodo principal de SAP HANA.
- Continúe con la instalación de los nodos de trabajo de SAP HANA.

#### <a name="install-sap-hana-in-a-scale-out-configuration"></a>Instalación de SAP HANA en una configuración de escalado horizontal
Se implementa la infraestructura de máquina virtual de Azure, y han finalizado todos los preparativos. Ahora, para instalar las configuraciones de escalado horizontal de SAP HANA, siga estos pasos:

- Instale el nodo maestro de SAP HANA según la documentación de SAP.
- *Después de la instalación, cambie el archivo global.ini y agregue el parámetro ' basepath_shared = n ' al archivo global.ini*. Este parámetro permite SAP HANA que se ejecutan en escalabilidad horizontal sin volúmenes / Hana/log y/Hana/Data compartido entre los nodos. Para obtener más información, consulte [SAP nota #2080991](https://launchpad.support.sap.com/#/notes/2080991).
- Después de cambiar el parámetro global.ini, reinicie la instancia de SAP HANA.
- Agregue nodos de trabajo adicionales. Para obtener más información, consulte [Guía de administración de SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US/0d9fe701e2214e98ad4f8721f6558c34.html). Especifique la red interna para la comunicación entre nodo de SAP HANA durante la instalación o posteriormente mediante, por ejemplo, el local hdblcm. Para obtener más información, consulte [SAP nota #2183363](https://launchpad.support.sap.com/#/notes/2183363). 

Después de esta rutina de configuración, la configuración de escalado horizontal que instaló usa discos no compartidos para ejecutar/Hana/Data y/Hana/log. El volumen/hana/shared se coloca en la alta disponibilidad recursos compartidos de NFS.


## <a name="sap-hana-dynamic-tiering-20-for-azure-virtual-machines"></a>SAP HANA 2.0 niveles dinámica para máquinas virtuales de Azure

Además de las certificaciones de SAP HANA en Azure máquinas virtuales de la serie M, SAP HANA 2.0 niveles dinámico (2.0 DT) también es compatible con Microsoft Azure. Para obtener vínculos a documentación dinámica los niveles de SAP HANA, consulte la sección "Vínculos a DT 2.0 documentación" más adelante en este artículo. No hay ninguna diferencia en la instalación del producto ni el funcionamiento, por ejemplo, a través de SAP HANA Cockpit dentro de una máquina virtual de Azure, pero algunos elementos importantes son obligatorios para oficial de soporte técnico en Azure. Estos puntos clave se describen en las secciones siguientes. 

SAP HANA DT 2.0 no es compatible con SAP BW o S4HANA. Ahora, los casos de uso principales son aplicaciones nativas de HANA.


### <a name="overview"></a>Información general

La siguiente imagen proporciona información general de soporte técnico de DT 2.0 en Microsoft Azure. Siga estos requisitos obligatorios para cumplir con la certificación oficial:

- DT 2.0 debe instalarse en una máquina virtual de Azure dedicada. No se puede ejecutar en la misma máquina virtual donde se ejecuta SAP HANA.
- SAP HANA y las máquinas virtuales de DT 2.0 deben implementarse dentro de la misma red virtual de Azure.
- Deben implementarse el SAP HANA y las máquinas virtuales de DT 2.0 con Azure Accelerated Networking habilitado.
- El tipo de almacenamiento para las máquinas virtuales 2.0 de DT debe ser almacenamiento premium de Azure.
- Varios discos de Azure deben asociarse a la máquina virtual 2.0 de DT.
- Creación de un software RAID o volumen seccionado, ya sea a través de LVM o mdadm, es necesario mediante la creación de bandas en los discos de Azure.

Las secciones siguientes proporcionan más información.

![Introducción a la arquitectura de SAP HANA DT 2.0](media/hana-vm-operations/hana-dt-20.PNG)



### <a name="dedicated-azure-vm-for-sap-hana-dt-20"></a>Máquina virtual de Azure dedicada para DT 2.0 de SAP HANA

En Azure IaaS, DT 2.0 solo se admite en una máquina virtual dedicada. DT 2.0 no tiene permiso para ejecutarse en la misma máquina virtual de Azure donde se está ejecutando la instancia de HANA. Inicialmente, se pueden usar dos tipos de máquina virtual para ejecutar SAP HANA DT 2.0:

- M64-32ms 
- E32sv3 

Para obtener descripciones de tipo de máquina virtual, consulte [tamaños de máquina virtual optimizada para memoria](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory).

Dada la idea básica de DT 2.0, que se encuentra sobre la descarga de datos semiactivos para ahorrar costos, tiene sentido usar tamaños de máquina virtual correspondientes. No hay ninguna regla estricta para las combinaciones posibles. Depende de la carga de trabajo específica del cliente.

La siguiente tabla muestra las configuraciones recomendadas.

| Tipo de máquina virtual de SAP HANA | Tipo de máquina virtual de DT 2.0 |
| --- | --- | 
| M128ms | M64-32ms |
| M128s | M64-32ms |
| M64ms | E32sv3 |
| M64s | E32sv3 |


Todas las combinaciones de máquinas virtuales de la serie M certificadas para SAP HANA con DT 2.0 las máquinas virtuales admitidas, como M64-32ms estándar y E32sv3, son posibles.


### <a name="azure-networking-and-sap-hana-dt-20"></a>Redes de Azure y DT 2.0 de SAP HANA

Instalar DT 2.0 en una máquina virtual dedicada requiere el rendimiento de la red entre la VM de DT 2.0 y la VM de SAP HANA con un mínimo de 10 GB. Como resultado, es obligatorio para colocar todas las máquinas virtuales dentro de la misma red virtual de Azure y habilitar Azure Accelerated Networking.

Para obtener más información sobre Azure Accelerated Networking, consulte [crear una máquina virtual Linux con Accelerated Networking](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli).

### <a name="vm-storage-for-sap-hana-dt-20"></a>Almacenamiento de máquinas virtuales para SAP HANA DT 2.0

Según las directrices de prácticas recomendadas de DT 2.0, el rendimiento de E/S de disco mínimo es 50 MB/s por núcleo físico. Mirar la especificación de los dos tipos de máquina virtual de Azure, que son compatibles para DT 2.0, el límite de rendimiento de E/S para la máquina virtual de disco máximo es:

- **E32sv3**:   768 MB/s, no almacenado en caché, lo que significa una proporción de 48 MB/s por núcleo físico
- **M64-32ms estándar**:  1000 MB/s, no almacenado en caché, lo que significa una proporción de 62,5 MB/s por núcleo físico

Es necesario adjuntar varios discos de Azure a la máquina virtual 2.0 de DT y crear un RAID por software mediante la creación de bandas en el nivel de sistema operativo para lograr el límite máximo de rendimiento de disco por máquina virtual. Un único disco de Azure no puede proporcionar el rendimiento hasta alcanzar el límite máximo de la máquina virtual. Azure premium storage es obligatoria para la ejecución DT 2.0. 

- Para obtener más información acerca de los tipos de disco de Azure disponibles, consulte [seleccione un tipo de disco para máquinas virtuales Windows de IaaS de Azure](../../windows/disks-types.md).
- Para obtener más información sobre cómo crear un RAID por software a través de mdadm, consulte [configurar software RAID en Linux](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid).
- Para obtener más información sobre cómo configurar LVM para crear un volumen seccionado para el máximo rendimiento, consulte [configuración del LVM en una VM Linux en Azure](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm).

Según los requisitos de tamaño, existen diferentes opciones para alcanzar el máximo rendimiento de una máquina virtual. Estas son las configuraciones de disco para volúmenes de datos posibles para cada tipo de máquina virtual de DT 2.0 a fin de alcanzar el límite superior de rendimiento para las máquinas virtuales. La máquina virtual de E32sv3 se considera un nivel de entrada para las cargas de trabajo más pequeños. Si no es lo suficientemente rápido, podría ser necesario cambiar el tamaño de la máquina virtual para M64-32ms estándar.

Dado que la máquina virtual de M64-32ms estándar tiene una gran cantidad de memoria, la carga de E/S no es posible que alcance el límite, especialmente para cargas de trabajo intensivas de lectura. Menos discos en el conjunto de bandas podrían ser suficientes según la carga de trabajo específica del cliente. Para que sea seguro, las siguientes configuraciones de disco se eligieron para garantizar el máximo rendimiento.


| SKU de la máquina virtual | Configuración de disco 1 | Configuración del disco 2 | Configuración de disco 3 | Configuración de disco 4 | Configuración de disco 5 | 
| ---- | ---- | ---- | ---- | ---- | ---- | 
| M64-32ms | 4 x P50 -> 16 TB | 4 x P40 -> 8 TB | 5 x P30 -> 5 TB | 7 x P20 -> 3,5 TB | 8 x P15 -> 2 TB | 
| E32sv3 | 3 x P50 -> 12 TB | 3 x P40 -> 6 TB | 4 x P30 -> 4 TB | 5 x P20 -> 2,5 TB | 6 x P15 -> 1,5 TB | 


Especialmente si la carga de trabajo es de lectura intensiva, la activación de la configuración de "solo lectura" de la caché de host de Azure que se recomienda para los volúmenes de datos de software de base de datos podría aumentar el rendimiento de E/S. El registro de transacciones, la caché de disco host de Azure debe ser "none". 

El punto de partida que recomendamos para el tamaño del volumen de registro es un método heurístico de 15 por ciento del tamaño de datos. Para crear el volumen del registro, utilice tipos de disco de Azure diferente dependiendo de los requisitos de costo y rendimiento. Para el volumen del registro, se requiere alto rendimiento de E/S. 

Si usa la máquina virtual, escriba M64-32ms estándar, se recomienda que habilite [Acelerador de escritura](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator). Acelerador de escritura es una característica de Azure que proporciona la latencia de escritura en disco óptimo para el registro de transacciones. Está disponible sólo para la serie M. Hay algunos elementos que deben considerarse, como el número máximo de discos por tipo de máquina virtual. Para obtener más información sobre el Acelerador de escritura, consulte [habilitar el Acelerador de escritura](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator).


En la tabla siguiente se muestra algunos ejemplos que le ayudarán a tamaño del volumen de registro.

| tipo de disco y el tamaño del volumen de datos | Iniciar configuración de tipo de volumen y disco 1 | Iniciar configuración de tipo de volumen y disco 2 |
| --- | --- | --- |
| 4 x P50 -> 16 TB | 5 x P20 -> 2,5 TB | 3 x P30 -> 3 TB |
| 6 x P15 -> 1,5 TB | 4 x P6 -> 256 GB | 1 x P15 -> 256 GB |


Al igual que el escalado horizontal de SAP HANA, el directorio/Hana/Shared debe compartirse entre la VM de SAP HANA y la máquina virtual 2.0 de DT. Se recomienda que utilice la misma arquitectura en cuanto a escalabilidad horizontal de SAP HANA mediante el uso de máquinas virtuales dedicadas, que actúan como un servidor NFS de alta disponibilidad. Para proporcionar un volumen compartido de copia de seguridad, use el diseño idéntico. Pero es usted decide si es necesaria la alta disponibilidad, o si es suficiente para usar una máquina virtual dedicada con suficiente capacidad de almacenamiento para que actúe como un servidor de copia de seguridad.



### <a name="links-to-dt-20-documentation"></a>Vínculos a la documentación de DT 2.0 

- [Instalación dinámica los niveles de SAP HANA y Guía de actualización](https://help.sap.com/viewer/88f82e0d010e4da1bc8963f18346f46e/2.0.03/en-US)
- [Recursos y tutoriales dinámicas los niveles de SAP HANA](https://help.sap.com/viewer/fb9c3779f9d1412b8de6dd0788fa167b/2.0.03/en-US)
- [SAP HANA dinámica los niveles como prueba de concepto](https://blogs.sap.com/2017/12/08/sap-hana-dynamic-tiering-delivering-on-low-tco-with-impressive-performance/)
- [SAP HANA 2.0 SPS 02 dynamic tiering enhancements](https://blogs.sap.com/2017/07/31/sap-hana-2.0-sps-02-dynamic-tiering-enhancements/) (Mejoras de Dynamic Tiering de SAP HANA 2.0 SPS 02)




## <a name="operations-for-deploying-sap-hana-on-azure-vms"></a>Operaciones de implementación de SAP HANA en máquinas virtuales de Azure
En las siguientes secciones se describen algunas de las operaciones relacionadas con la implementación de sistemas SAP HANA en máquinas virtuales de Azure.

### <a name="back-up-and-restore-operations-on-azure-vms"></a>Operaciones de copia de seguridad y restauración en máquinas virtuales Azure
En los documentos siguientes se describe cómo realizar la copia de seguridad y la restauración de una implementación de SAP HANA:

- [SAP HANA: información general sobre copias de seguridad](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [Azure Backup de SAP HANA en el nivel de archivo](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [Copia de seguridad de SAP HANA basada en instantáneas de almacenamiento](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)


### <a name="start-and-restart-vms-that-contain-sap-hana"></a>Iniciar y reiniciar máquinas virtuales que contienen SAP HANA
Una característica fundamental de la nube pública de Azure es que solo se le cobra por los minutos de computación. Por ejemplo, cuando se apaga una máquina virtual que ejecuta SAP HANA, factura solo los costos de almacenamiento durante ese tiempo. Otra característica está disponible cuando se especifican las direcciones IP estáticas de las máquinas virtuales de la implementación inicial. Cuando se reinicia una máquina virtual con SAP HANA, lo hace con sus direcciones IP anteriores. 


### <a name="use-saprouter-for-sap-remote-support"></a>Usar SAPRouter para la compatibilidad remota de SAP
Si tiene una conexión de sitio a sitio entre sus ubicaciones locales y Azure y ejecuta los componentes de SAP, probablemente ya está ejecutando SAProuter. En este caso, siga estos pasos para obtener compatibilidad remota:

- Mantener la dirección IP estática y privada de la máquina virtual que hospeda SAP HANA en la configuración de SAProuter.
- Configurar el NSG de la subred que hospeda la máquina virtual de HANA para permitir el tráfico a través del puerto TCP/IP 3299.

Si se conecta a Azure a través de internet y no tiene un enrutador SAP para la máquina virtual con SAP HANA, instale el componente. Instale SAProuter en una máquina virtual independiente en la subred de administración. 

La imagen siguiente muestra un esquema aproximado de implementación de SAP HANA sin una conexión de sitio a sitio y con SAProuter:

![Esquema de implementación aproximado para SAP HANA sin conexión de sitio a sitio y SAPRouter](media/hana-vm-operations/hana-simple-networking3.PNG)

Asegúrese de instalar SAProuter en una máquina virtual independiente y no en la VM de JumpBox. La máquina virtual independiente debe tener una dirección IP estática. Para conectar la instancia de SAProuter con la instancia de SAProuter hospedada en SAP, póngase en contacto con SAP para una dirección IP. La instancia de SAProuter hospedada en SAP es el equivalente de la instancia de SAProuter que se instala en la máquina virtual. Utilice la dirección IP de SAP para configurar la instancia de SAProuter. En la configuración, el único puerto necesario es el puerto TCP 3299.

Para obtener más información sobre cómo configurar y mantener las conexiones de asistencia remota a través de SAProuter, consulte [documentación de SAP](https://support.sap.com/en/tools/connectivity-tools/remote-support.html).

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>Alta disponibilidad con SAP HANA en máquinas virtuales nativas de Azure
Si ejecuta SUSE Linux Enterprise Server para SAP Applications 12 SP1 o posterior, puede establecer un clúster de Pacemaker con dispositivos STONITH. Usar los dispositivos para establecer una configuración de SAP HANA que utilice la replicación sincrónica con conmutación automática por error y replicación de sistema de HANA. Para obtener más información sobre el procedimiento de instalación, consulte [Guía de alta disponibilidad de SAP HANA para máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).
