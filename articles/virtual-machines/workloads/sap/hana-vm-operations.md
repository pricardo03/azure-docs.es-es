---
title: Configuraciones y operaciones de infraestructura de SAP HANA en Azure | Microsoft Docs
description: Guía de operaciones para los sistemas SAP HANA que se implementan en Azure Virtual Machines.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: juergent
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
ms.author: msjuergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d716a27cc2b4879451a8d5edbca46ca1bbfeaf40
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "52968994"
---
# <a name="sap-hana-infrastructure-configurations-and-operations-on-azure"></a>Configuraciones y operaciones de infraestructura de SAP HANA en Azure
En este documento se proporcionan instrucciones para configurar la infraestructura de Azure y sobre el funcionamiento de los sistemas SAP HANA que se implementaron en máquinas virtuales nativas de Azure. En el documento también se incluye información sobre la configuración de la escalabilidad horizontal de SAP HANA para la SKU de máquinas virtuales M128s. Este documento no pretende reemplazar ninguna documentación estándar de SAP, incluido el contenido siguiente:

- [Guía de administración de SAP](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [Guías de instalación de SAP](https://service.sap.com/instguides)
- [Notas de SAP](https://sservice.sap.com/notes)

## <a name="prerequisites"></a>Requisitos previos
Para utilizar esta guía, necesita un conocimiento básico de los siguientes componentes de Azure:

- [Máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Redes de azure y Azure Virtual Network](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure Storage](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Para obtener más información acerca de SAP NetWeaver y otros componentes de SAP en Azure, consulte la sección [SAP en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) de la [documentación de Azure](https://docs.microsoft.com/azure/).

## <a name="basic-setup-considerations"></a>Consideraciones básicas sobre la configuración
En las siguientes secciones se describen las consideraciones de configuración básicas para implementar sistemas SAP HANA en máquinas virtuales de Azure.

### <a name="connect-into-azure-virtual-machines"></a>Conectarse a Azure Virtual Machines
Como se documenta en la [guía de planeamiento de Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide), existen dos métodos básicos para conectarse a máquinas virtuales de Azure:

- Conéctese a través de Internet y de los puntos de conexión públicos en una máquina virtual de Jump o en la máquina virtual que ejecuta SAP HANA.
- Conéctese a través de una [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) o de Azure [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

La conectividad de sitio a sitio a través de VPN o ExpressRoute es necesaria para los escenarios de producción. Este tipo de conexión también es necesario para escenarios de no producción que se incorporan en escenarios de producción cuando se usa el software SAP. La imagen siguiente muestra un ejemplo de conectividad entre sitios:

![Conectividad entre sitios](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choose-azure-vm-types"></a>Elegir tipos de máquina virtual de Azure
Los tipos de máquina virtual de Azure que pueden utilizarse para escenarios de producción se muestran en la [documentación de SAP para IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). En el caso de los escenarios que no son de producción, existe una variedad más amplia de tipos de máquinas virtuales de Azure disponible.

>[!NOTE]
> Para escenarios no de producción, use los tipos de máquinas virtuales que se enumeran en la [nota de SAP 1928533](https://launchpad.support.sap.com/#/notes/1928533). Para el uso de máquinas virtuales de Azure en escenarios de producción, compruebe las máquinas virtuales certificadas para SAP HANA en el documento publicado por SAP con la [lista de plataformas de IaaS certificadas](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure).

Implemente las máquinas virtuales en Azure mediante:

- Azure Portal.
- Los cmdlets de Azure PowerShell.
- La CLI de Azure.

También puede implementar una plataforma SAP HANA completamente instalada en los servicios de máquina virtual de Azure a través de la [plataforma en la nube de SAP](https://cal.sap.com/). El proceso de instalación se describe en [Implementación de SAP S/4HANA o BW/4HANA en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h), o con la automatización publicada [aquí](https://github.com/AzureCAT-GSI/SAP-HANA-ARM).

### <a name="choose-azure-storage-type"></a>Elegir un tipo de Azure Storage
Azure proporciona dos tipos de almacenamiento adecuados para máquinas virtuales de Azure que ejecutan SAP HANA:

- [Azure Standard Storage](https://docs.microsoft.com/azure/virtual-machines/windows/standard-storage)
- [Azure Premium Storage](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage)

Azure ofrece dos métodos de implementación de discos duros virtuales en Azure Standard Storage y Azure Premium Storage. Si el escenario global lo permite, aproveche las implementaciones de [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/).

Para obtener una lista de tipos de almacenamiento y sus Acuerdos de Nivel de Servicio sobre IOPS y rendimiento del almacenamiento, revise la [documentación de Azure para Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/).

### <a name="configuring-the-storage-for-azure-virtual-machines"></a>Configuración del almacenamiento para máquinas virtuales de Azure

Hasta ahora nunca ha tenido que preocuparse sobre los subsistemas de E/S y sus funcionalidades. El motivo era que el proveedor de las aplicaciones tenía que asegurarse de que se cumplieran los requisitos de almacenamiento mínimo para SAP HANA. Mientras compila la infraestructura de Azure por su cuenta, también debe tener en cuenta algunos de esos requisitos. También debe conocer los requisitos de configuración sugeridos en las secciones siguientes. O para los casos en los que va a configurar las máquinas virtuales en las que desee ejecutar SAP HANA. Algunas de las características que se piden comportan la necesidad de:

- Habilitar el volumen de lectura/escritura en **/hana/log** de 250 MB/s como mínimo con tamaños de E/S de 1 MB
- Habilitar la actividad de lectura de al menos 400 MB/s para **/hana/data** con tamaños de E/S de 16 MB y 64 MB
- Habilitar la actividad de escritura de al menos 250 MB/s para **/hana/data** con tamaños de E/S de 16 MB y 64 MB

Debido a que la baja latencia de almacenamiento es fundamental para los sistemas DBMS, incluso estos DBMS, como SAP HANA, mantienen los datos en memoria. La ruta crítica en el almacenamiento normalmente está relacionada con las escrituras del registro de transacciones de los sistemas DBMS. Sin embargo, también las operaciones como la escritura de puntos de retorno o la carga de datos en memoria después de la recuperación tras un bloqueo pueden ser críticas. Por ello es obligatorio aprovechar Azure Premium Disks para los volúmenes **/hana/data** y **/hana/log**. Para lograr el rendimiento mínimo de **/hana/log** y **/hana/data** como desea SAP, tiene que compilar un RAID 0 utilizando MDADM o LVM en varios discos de Azure Premium Storage, y usar los volúmenes RAID como volúmenes de **/hana/data** y **/hana/log**. Como tamaños de franja para RAID 0 la recomendación es utilizar:

- 64 KB o 128 KB para  **/hana/data**
- 32 KB para **/hana/log**

> [!NOTE]
> No es necesario configurar ningún nivel de redundancia con volúmenes RAID, ya que Azure Premium Storage y Azure Standard Storage mantienen tres imágenes de un disco duro virtual. El uso de un volumen RAID tiene como finalidad configurar volúmenes que proporcionen un rendimiento de E/S suficiente.

La acumulación de un número de discos duros virtuales de Azure bajo un RAID es acumulativo por parte de un IOPS y del rendimiento de almacenamiento. Por lo tanto, si coloca una RAID 0 en P30 x 3 discos de Azure Premium Storage, debe proporcionarle tres veces el IOPS y tres veces el rendimiento de almacenamiento de un solo disco P30 de Azure Premium Storage.

Estas recomendaciones de almacenamiento en caché suponen que las características de E/S para SAP HANA que se enumeran son como las siguientes:

- Apenas hay carga de trabajo de lectura en los archivos de datos de HANA. Las excepciones son las operaciones de E/S de tamaño grande después del reinicio de la instancia HANA o cuando se cargan datos en HANA. Otro caso de operaciones de E/S de lectura de mayor tamaño con archivos de datos pueden ser las copias de seguridad de base de datos de HANA. Como resultado, el almacenamiento en caché de lectura no tiene sentido ya que, en la mayoría de los casos, todos los volúmenes de archivos de datos tienen que leerse por completo.
- La escritura en los archivos de datos se produce en ráfagas según los puntos de retorno de HANA y la recuperación tras bloqueos de HANA. La escritura de los puntos de retorno es asincrónica y no bloquea las transacciones de usuario. La escritura de datos durante la recuperación tras bloqueo es esencial en el rendimiento a fin de lograr que el sistema responda de nuevo rápidamente. Sin embargo, la recuperación tras bloqueo debería constituir una situación excepcional.
- Apenas hay lecturas de los archivos de puesta al día de HANA. Las operaciones de E/S grandes al realizar copias de seguridad del registro de transacciones, la recuperación tras bloqueo o en la fase de reinicio de una instancia de HANA constituyen excepciones.  
- La carga principal en el caso del archivo de registro de puesta al día SAP HANA la constituyen las escrituras. Según la naturaleza de la carga de trabajo, puede tener operaciones de E/S de solo 4 KB o llegar a superar 1 MB. La latencia de las escrituras en relación con el registro de puesta al día SAP HANA es esencial para el rendimiento.
- Todas las escrituras deben conservarse en el disco de forma confiable.

Como resultado de estos patrones de E/S observados por SAP HANA, debe establecer el almacenamiento en caché de los diferentes volúmenes con Azure Premium Storage como sigue:

- **/hana/data**: sin almacenamiento en caché
- **/hana/log**: sin almacenamiento en caché; excepción con la serie M (vea más adelante en este documento)
- **/hana/shared**: almacenamiento en caché de lecturas


Tenga también en cuenta el rendimiento global de E/S de la máquina virtual al elegir una máquina virtual o determinar su tamaño. El rendimiento general del almacenamiento de las máquinas virtuales está documentado en el artículo [Tamaños de máquina virtual optimizada para memoria](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory).

#### <a name="linux-io-scheduler-mode"></a>Modo de programador de E/S de Linux
Linux tiene varios modos diferentes de programación de E/S. Una recomendación común de los proveedores de Linux y SAP consiste en establecer el modo de programador de E/S para los volúmenes de disco fuera del modo **cfq** en el modo **noop**. Los detalles se incluyen en la [nota de SAP n.º 1984798](https://launchpad.support.sap.com/#/notes/1984787). 


#### <a name="storage-solution-with-azure-write-accelerator-for-azure-m-series-virtual-machines"></a>Solución de almacenamiento con Acelerador de escritura de Azure para máquinas virtuales de serie M de Azure
El Acelerador de escritura de Azure es una funcionalidad que se implementa exclusivamente para las máquinas virtuales de la serie M de Azure. Como el nombre indica, el propósito de la funcionalidad es mejorar la latencia de E/S de las escrituras en Azure Premium Storage. Para SAP HANA, el Acelerador de escritura se supone que debe usarse solo en el volumen **/hana/log**. Por lo tanto, las configuraciones que se han mostrado hasta ahora tienen que cambiarse. El cambio principal es el desglose entre **/hana/data** y **/hana/log** para poder usar el Acelerador de escritura de Azure solo en el volumen **/hana/log**. 

> [!IMPORTANT]
> La certificación de SAP HANA para las máquinas virtuales de serie M de Azure es exclusivamente con el Acelerador de escritura de Azure para el volumen **/hana/log**. Como resultado, las implementaciones de SAP HANA en un escenario de producción en máquinas virtuales de la serie M de Azure se espera que estén configuradas con el Acelerador de escritura de Azure para el volumen **/hana/log**.  

> [!NOTE]
> Para escenarios de producción, compruebe si un determinado tipo de máquina virtual es compatible con SAP HANA de SAP en la [documentación de SAP para IaaS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).

Las configuraciones recomendadas tiene el siguiente aspecto:

| SKU de la máquina virtual | RAM | Máx. E/S de VM<br /> Throughput | /hana/data | /hana/log | /hana/shared | /root volume | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | --- | -- |
| M32ts | 192 GiB | 500 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M32ls | 256 GiB | 500 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P20 |
| M64ls | 512 GB | 1000 MB/s | 3 x P20 | 2 x P20 | 1 x P20 | 1 x P6 | 1 x P6 |1 x P30 |
| M64s | 1000 GiB | 1000 MB/s | 4 x P20 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 |2 x P30 |
| M64ms | 1750 GiB | 1000 MB/s | 3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 3 x P30 |
| M128s | 2000 GiB | 2000 MB/s |3 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P40 |
| M128ms | 3800 GiB | 2000 MB/s | 5 x P30 | 2 x P20 | 1 x P30 | 1 x P6 | 1 x P6 | 2 x P50 |

Compruebe si el rendimiento del almacenamiento para los diferentes volúmenes sugeridos se adaptará a la carga de trabajo que se va a ejecutar. Si la carga de trabajo requiere volúmenes mayores para **/hana/data** y **/hana/log**, tiene que aumentar el número de discos duros virtuales de Azure Premium Storage. Ajustar el tamaño de un volumen con más discos duros virtuales de los que se enumeran, aumentará el IOPS y el rendimiento de E/S dentro de los límites del tipo de máquina virtual de Azure.

El Acelerador de escritura de Azure solo funciona en conjunto con [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/). Por lo que al menos los discos de Azure Premium Storage que forman el volumen **/hana/log** tienen que implementarse como discos administrados.

Hay límites en los discos duros virtuales de Azure Premium Storage por máquina virtual que el Acelerador de escritura de Azure puede admitir. Los límites actuales son:

- 16 VHD para una máquina virtual M128xx
- 8 VHD para una máquina virtual M64xx
- 4 discos duros virtuales para una máquina virtual M32xx

Puede encontrar instrucciones más detalladas sobre cómo habilitar el Acelerador de escritura de Azure en el artículo [Write Accelerator](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator) (Acelerador de escritura).

En la misma documentación puede encontrar información más detallada y las restricciones para el Acelerador de escritura de Azure.


#### <a name="cost-conscious-azure-storage-configuration"></a>Configuración de Azure Storage con control de costo
En la tabla siguiente se muestra una configuración de tipos de VM que los clientes suelen usar para hospedar SAP HANA en máquinas virtuales de Azure. Puede haber algunos tipos de máquina virtual que no cumplan todos los criterios mínimos para SAP HANA o no se admitan de forma oficial con SAP HANA en SAP. Pero hasta ahora parece que esas máquinas virtuales presentan un rendimiento adecuado en escenarios que no son de producción. 

> [!NOTE]
> Para escenarios de producción, compruebe si un determinado tipo de máquina virtual es compatible con SAP HANA de SAP en la [documentación de SAP para IaaS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html).


| SKU de la máquina virtual | RAM | Máx. E/S de VM<br /> Throughput | /hana/data y /hana/log<br /> seccionado con LVM o MDAADM | /hana/shared | /root volume | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | -- |
| DS14v2 | 128 GB | 768 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E16v3 | 128 GB | 384 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S15 |
| E32v3 | 256 GiB | 768 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| E64v3 | 443 GiB | 1200 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| GS5 | 448 GiB | 2000 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S30 |
| M32ts | 192 GiB | 500 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M32ls | 256 GiB | 500 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 | 1 x S20 |
| M64ls | 512 GB | 1000 MB/s | 3 x P20 | 1 x S20 | 1 x S6 | 1 x S6 |1 x S30 |
| M64s | 1000 GiB | 1000 MB/s | 2 x P30 | 1 x S30 | 1 x S6 | 1 x S6 |2 x S30 |
| M64ms | 1750 GiB | 1000 MB/s | 3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 3 x S30 |
| M128s | 2000 GiB | 2000 MB/s |3 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S40 |
| M128ms | 3800 GiB | 2000 MB/s | 5 x P30 | 1 x S30 | 1 x S6 | 1 x S6 | 2 x S50 |


Discos recomendados para los tipos de máquinas virtuales más pequeños con un exceso de tamaño de los volúmenes de 3 x P20 con respecto a las recomendaciones de espacio según las [notas del producto sobre el almacenamiento de TDI de SAP](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html). No obstante, la elección que se muestra en la tabla se realizó para proporcionar un rendimiento del disco suficiente para SAP HANA. Si necesitar realizar cambios en el volumen **/hana/backup**, cuyo tamaño se estableció para mantener copias de seguridad que representan el doble del volumen de memoria, puede realizar los ajustes que requiera.   
Compruebe si el rendimiento del almacenamiento para los diferentes volúmenes sugeridos se adaptará a la carga de trabajo que se va a ejecutar. Si la carga de trabajo requiere volúmenes mayores para **/hana/data** y **/hana/log**, tiene que aumentar el número de discos duros virtuales de Azure Premium Storage. Ajustar el tamaño de un volumen con más discos duros virtuales de los que se enumeran, aumentará el IOPS y el rendimiento de E/S dentro de los límites del tipo de máquina virtual de Azure. 

> [!NOTE]
> Las configuraciones anteriores NO se beneficiarían del [Acuerdo de nivel de servicio de máquinas virtuales de Azure para una máquina virtual única](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_6/) debido a que utilizan una combinación de Azure Premium Storage y Azure Standard Storage. Sin embargo, la selección se ha elegido para optimizar los costos. Debe elegir el almacenamiento Premium para todos los discos por encima del que aparece como Azure Standard Storage (Sxx) para que la configuración de máquina virtual sea compatible con el Acuerdo de Nivel de Servicio de máquina virtual de Azure único.


> [!NOTE]
> Las recomendaciones de configuración de discos que se han indicado se dirigen a los requisitos mínimos que SAP expresa a sus proveedores de infraestructura. En las implementaciones reales de clientes y en los escenarios de carga de trabajo, se encontraron situaciones en las que estas recomendaciones aún no proporcionaban suficientes funcionalidades. Estas podrían ser situaciones en las que un cliente necesitara una recarga más rápida de los datos después de un reinicio de HANA o en las que las configuraciones de copia de seguridad requirieran un mayor ancho de banda para el almacenamiento. Otros casos incluían **/hana/log**, en los que 5000 IOPS no fueron suficientes para la carga de trabajo específica. Por lo tanto, tome estas recomendaciones como punto de partida y adáptelas en función de los requisitos de la carga de trabajo.
>  

### <a name="set-up-azure-virtual-networks"></a>Configurar Azure Virtual Network
Si tiene conectividad de sitio a sitio a Azure por medio de VPN o de ExpressRoute, debe tener como mínimo una red de Azure Virtual Network conectada a través de una puerta de enlace virtual al circuito VPN o ExpressRoute. En las implementaciones sencillas, la puerta de enlace virtual puede implementarse en una subred de la instancia de Azure Virtual Network (VNet) que también hospeda las instancias de SAP HANA. Para instalar SAP HANA, debe crear dos subredes adicionales en la instancia de Azure Virtual Network. Una subred hospeda las máquinas virtuales para ejecutar las instancias de SAP HANA. La otra subred ejecuta Jumpbox o máquinas virtuales de administración para hospedar SAP HANA Studio, otro software de administración o el software de la aplicación.

> [!IMPORTANT]
> Además de por la funcionalidad y, lo que es más importante, por motivos de rendimiento, no se puede configurar [Aplicaciones virtuales de red de Azure](https://azure.microsoft.com/solutions/network-appliances/) en la ruta de comunicación entre la aplicación de SAP y la capa DBMS de un sistema SAP basado en SAP NetWeaver, Hybris o S/4HANA. La comunicación entre la capa de la aplicación de SAP y la capa de DBMS debe ser directa. La restricción no incluye [reglas ASG ni NSG de Azure](https://docs.microsoft.com/azure/virtual-network/security-overview), siempre y cuando dichas reglas ASG y NSG permitan una comunicación directa. Más escenarios donde no se admiten los NVA se dan en las rutas de comunicación entre las máquinas virtuales de Azure que representan los nodos de clúster de Linux Pacemaker y los dispositivos SBD, según se describe en [Alta disponibilidad para SAP NetWeaver en máquinas virtuales de Azure en SUSE Linux Enterprise Server para SAP Applications](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse). O bien, en las rutas de comunicación establecidas entre las máquinas virtuales de Azure y Windows Server SOFS configuradas como se describe en [Agrupación de una instancia de ASCS/SCS de SAP en un clúster de conmutación por error de Windows con un recurso compartido de archivos en Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share). Las aplicaciones virtuales de red en las rutas de comunicación pueden duplicar fácilmente la latencia de red entre dos socios de comunicación y restringir el rendimiento en las rutas críticas entre la capa de la aplicación de SAP y la capa de DBMS. En algunos escenarios que se han observado con los clientes, las aplicaciones virtuales de red pueden ocasionar que los clústeres Pacemaker Linux produzcan un error cuando las comunicaciones entre los nodos del clúster de Linux Pacemaker necesiten comunicarse con su dispositivo SBD mediante una NVA.  
> 

> [!IMPORTANT]
> Otro diseño que **NO** se admite es la segregación de la capa de la aplicación de SAP ni la capa de DBMS en diferentes redes virtuales de Azure que no están [emparejadas](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) entre sí. Se recomienda separar la capa de la aplicación de SAP y la capa de DBMS con subredes dentro de una red virtual de Azure, en lugar de usar diferentes redes virtuales de Azure. Si decide no seguir la recomendación y, en su lugar, separa las dos capas en redes virtuales diferentes, las dos redes virtuales deben estar [emparejadas](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview). Tenga en cuenta que el tráfico entre dos redes virtuales de Azure [emparejadas](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) está sujeto a costes de transferencia. Debido al intercambio de un gran volumen de datos en terabytes entre la capa de la aplicación de SAP y la capa de DBMS, pueden acumularse costos sustanciales si la capa de la aplicación de SAP y la capa de DBMS se separan entre dos redes virtuales de Azure emparejadas. 

Al instalar las máquinas virtuales para ejecutar SAP HANA, estas necesitan lo siguiente:

- Dos NIC virtuales instaladas: una para conectarse a la subred de administración y la otra para conectarse ya sea desde la red local o desde otras redes a la instancia de SAP HANA de la máquina virtual de Azure.
- Direcciones IP privadas estáticas implementadas para ambas NIC virtuales.

> [!NOTE]
> Debería asignar direcciones IP estáticas mediante Azure a NIC virtuales individuales. No debería asignar direcciones IP estáticas dentro del sistema operativo invitado a una NIC virtual. Algunos servicios de Azure como el servicio Azure Backup se basan en el hecho de que al menos el vNIC principal está establecido en DHCP y no en direcciones IP estáticas. Consulte también el documento [Solución de problemas de copia de seguridad de máquinas virtuales de Azure](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking). Si necesita asignar varias direcciones IP estáticas a una máquina virtual, tiene que asignar varias NIC virtuales a una máquina virtual.
>
>

Sin embargo, para implementaciones permanentes, deberá crear una arquitectura de red del centro de datos virtual en Azure. Esta arquitectura recomienda la separación de la puerta de enlace de Azure Virtual Network que se conecta a entornos locales en una instancia independiente de Azure Virtual Network. Esta red virtual independiente debe hospedar todo el tráfico que sale a entornos locales o a Internet. Este enfoque permite implementar software para la auditoría y el registro de tráfico que entra en el centro de datos virtual de Azure en esta red virtual hub independiente. De esta forma, dispondrá de una red virtual que hospeda todo el software y las configuraciones relacionados con el tráfico de entrada y salida de la implementación de Azure.

En los artículos [Centro de datos virtual de Azure: una perspectiva de red](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter) y [Centro de datos virtual de Azure y el plano de control empresarial](https://docs.microsoft.com/azure/architecture/vdc/) se ofrece más información sobre el enfoque del centro de datos virtual y el diseño relacionado de Azure Virtual Network.


>[!NOTE]
>El tráfico que fluye entre una red virtual hub y otra spoke mediante [emparejamiento de redes virtuales de Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) supone [costos](https://azure.microsoft.com/pricing/details/virtual-network/) adicionales. Según estos costos, puede que deba plantearse la opción de asumir compromisos entre la ejecución de un diseño de red hub-and-spoke estricto y la ejecución de varias [puertas de enlace de Azure ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways) que conecta a los "radios" para omitir el emparejamiento de redes virtuales. Sin embargo, las puertas de enlace de Azure ExpressRoute también generan [costos](https://azure.microsoft.com/pricing/details/vpn-gateway/) adicionales. También puede incurrir en costos adicionales por el software de terceros que usa para el registro, la auditoría y la supervisión del tráfico de red. En función de los costos derivados del intercambio de datos mediante el emparejamiento de redes virtuales, por una parte, y de los costos que generan las puertas de enlace de Azure ExpressRoute adicionales y las licencias de software adicionales, por otra, puede que deba plantearse la microsegmentación dentro de una red virtual con el uso de subredes como unidad de aislamiento en lugar de utilizar redes virtuales.


Para obtener información general sobre los distintos métodos para asignar direcciones IP, consulte [Tipos de direcciones IP y métodos de asignación en Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm). 

Para máquinas virtuales que ejecutan SAP HANA, debe trabajar con direcciones IP estáticas asignadas. La razón es que algunos atributos de configuración de HANA hacen referencia a direcciones IP.

Los [grupos de seguridad de red (NSG) de Azure](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) se usan para dirigir el tráfico que se enruta a la instancia de SAP HANA o al Jumpbox. Los NSG y los posibles [grupos de seguridad de aplicaciones](https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups) se asocian a la subred de SAP HANA y a la subred de administración.

En la siguiente imagen se muestra un resumen de un esquema de implementación aproximado para SAP HANA según una arquitectura de red virtual tipo hub-and-spoke:

![Esquema de implementación aproximado de SAP HANA](media/hana-vm-operations/hana-simple-networking.PNG)

Para implementar SAP HANA en Azure sin una conexión de sitio a sitio, todavía debe blindar la instancia de SAP HANA desde la red pública de Internet y ocultarla detrás de un proxy de reenvío. En este escenario básico, la implementación depende de los servicios DNS integrados en Azure para resolver los nombres de host. En una implementación más compleja en la que se usan direcciones IP de acceso público, los servicios DNS integrados en Azure son especialmente importantes. Use los grupos de seguridad de red de Azure y las [aplicaciones virtuales de red de Azure](https://azure.microsoft.com/solutions/network-appliances/) para controlar y supervisar el enrutamiento desde Internet a la arquitectura de Azure Virtual Network. En la imagen siguiente se muestra un esquema aproximado de implementación de SAP HANA sin una conexión de sitio a sitio en una arquitectura de red virtual tipo hub-and-spoke:
  
![Esquema de implementación aproximado de SAP HANA sin una conexión de sitio a sitio](media/hana-vm-operations/hana-simple-networking2.PNG)
 

Se puede consultar otra descripción sobre cómo usar las aplicaciones virtuales de red de Azure para controlar y supervisar el acceso desde Internet sin la arquitectura de red virtual tipo hub-and-spoke en el artículo [Implementación de aplicaciones virtuales de red de alta disponibilidad](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha).


## <a name="configuring-azure-infrastructure-for-sap-hana-scale-out"></a>Configuración de la infraestructura de Azure para la escalabilidad horizontal de SAP HANA
Microsoft tiene una SKU de máquina virtual de la serie M certificada para una configuración de la escalabilidad horizontal de SAP HANA. El tipo de máquina virtual M128s obtuvo una certificación para una escalabilidad horizontal de hasta dieciséis nodos. Para realizar cambios en las certificaciones de escalabilidad horizontal de SAP HANA en máquinas virtuales de Azure, consulte la [lista de plataformas IaaS certificadas](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure).

Las versiones mínimas del sistema operativo para implementar configuraciones de escalabilidad horizontal en máquinas virtuales de Azure son:

- SUSE Linux 12 SP3
- Red Hat Linux 7.4

De la certificación de escalabilidad horizontal de dieciséis nodos

- Un nodo es el nodo maestro
- Un máximo de quince nodos son nodos de trabajo

>[!NOTE]
>En las implementaciones de escalabilidad horizontal de máquinas virtuales de Azure no existe la posibilidad de usar un nodo en espera.
>

Hay dos razones por las que no se puede configurar un nodo en espera:

- En este momento, Azure no tiene un servicio NFS nativo. Como resultado, los recursos compartidos de NFS deben configurarse con la ayuda de una funcionalidad de terceros.
- Ninguna de las configuraciones de NFS de terceros es capaz de satisfacer los criterios de latencia de almacenamiento para SAP HANA con sus soluciones implementadas en Azure.

Como resultado, los volúmenes **/hana/data** y **/hana/log** no se pueden compartir. La incapacidad de compartir estos volúmenes de un único nodo impide usar un nodo en espera de SAP HANA en una configuración de escalabilidad horizontal.

Como resultado, el diseño básico de un solo nodo en una configuración de escalabilidad horizontal será similar a la siguiente:

![Conceptos básicos de escalabilidad horizontal de un solo nodo](media/hana-vm-operations/scale-out-basics.PNG)

La configuración básica de un nodo de máquina virtual para escalabilidad horizontal de SAP HANA presenta este aspecto:

- Para **/hana/shared**, cree un clúster NFS de alta disponibilidad basado en SUSE Linux 12 SP3. Este clúster hospedará los recursos compartidos NFS de **/hana/shared** de la configuración de escalabilidad horizontal y SAP NetWeaver o BW/4HANA Central Services. La documentación para crear dicha configuración está disponible en el artículo [Alta disponibilidad para NFS en máquinas virtuales de Azure en SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs).
- Todos los demás volúmenes de disco **NO** se comparten entre los diferentes nodos y **NO** se basan en NFS. Las configuraciones de instalación y los pasos para la escalabilidad horizontal de instalaciones HANA con los recursos no compartidos **/hana/data** y **/hana/log** se proporcionan más adelante en este documento.

>[!NOTE]
>El clúster NFS de alta disponibilidad, como se muestra en los gráficos, hasta ahora, solo se admite con SUSE Linux. Más adelante se aconsejará una solución NFS de alta disponibilidad basada en Red Hat.

Los tamaños de los volúmenes para los nodos son los mismos que para la escalabilidad horizontal, salvo en el caso de **/hana/shared**. SKU de máquinas virtuales M128s, los tamaños y tipos sugeridos serán estos:

| SKU de la máquina virtual | RAM | Máx. E/S de VM<br /> Throughput | /hana/data | /hana/log | /root volume | /usr/sap | hana/backup |
| --- | --- | --- | --- | --- | --- | --- | --- |
| M128s | 2000 GiB | 2000 MB/s |3 x P30 | 2 x P20 | 1 x P6 | 1 x P6 | 2 x P40 |


Compruebe si el rendimiento del almacenamiento para los diferentes volúmenes sugeridos se adaptará a la carga de trabajo que se va a ejecutar. Si la carga de trabajo requiere volúmenes mayores para **/hana/data** y **/hana/log**, tiene que aumentar el número de discos duros virtuales de Azure Premium Storage. Ajustar el tamaño de un volumen con más discos duros virtuales de los que se enumeran, aumentará el IOPS y el rendimiento de E/S dentro de los límites del tipo de máquina virtual de Azure. También se aplica el Acelerador de escritura de Azure a los discos que forman el volumen **/hana/log**.
 
En el documento [SAP HANA TDI Storage Requirements](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) (Requisitos de almacenamiento de la integración adaptada de centros de datos de SAP HANA), se nombra una fórmula que define el tamaño del volumen **/hana/shared** para la escalabilidad horizontal hasta un tamaño de memoria de un único nodo de trabajo por cuatro nodos de trabajo.

Suponiendo que use una máquina virtual de Azure M128s para la escalabilidad horizontal en SAP HANA con 2 TB de memoria aproximadamente, las recomendaciones de SAP pueden resumirse como sigue:

- Un nodo maestro y hasta cuatro nodos de trabajo; el tamaño del volumen **/hana/shared** debe ser de 2 TB. 
- Un nodo maestro y de cinco a ocho nodos de trabajo; el tamaño de **/hana/shared** debe ser de 4 TB. 
- Un nodo maestro y de nueve a doce nodos de trabajo; el tamaño de **/hana/shared** debe ser de 6 TB. 
- Un nodo maestro y entre doce y quince nodos de trabajo; se requiere que proporcione un volumen **/hana/shared** con un tamaño de 8 TB.

El otro diseño importante que se muestra en los gráficos de la configuración de un único nodo para una máquina virtual de SAP HANA de escalabilidad horizontal es la red virtual, o mejor la configuración de subred. SAP recomienda encarecidamente una separación del tráfico orientado al cliente y a la aplicación de las comunicaciones entre los nodos HANA. Como se muestra en los gráficos, esto se logra al tener dos VNIC diferentes conectados a la máquina virtual. Ambos VNIC están en subredes diferentes, con dos direcciones IP distintas. Puede controlar el flujo del tráfico con reglas de enrutamiento mediante grupos de seguridad de red o rutas definidas por el usuario.

Especialmente en Azure, no hay ningún medio o método para exigir la calidad de servicio y las cuotas en VNIC específicos. Como resultado, la separación de las comunicaciones entre nodos y aquellas orientadas al cliente y a la aplicación no ofrece ninguna oportunidad de dar prioridad a un flujo de tráfico con respecto a otro. En su lugar, la separación sigue siendo una medida de seguridad al blindar las comunicaciones entre nodos de las configuraciones de escalabilidad horizontal.  

>[!IMPORTANT]
>SAP recomienda encarecidamente separar el tráfico de red hacia el cliente y la aplicación del tráfico entre nodos, como se describe en este documento. Por tanto, resulta muy conveniente establecer una arquitectura como la que se muestra en el último gráfico.
>

Desde el punto de vista de las redes, la arquitectura de red mínima requerida sería:

![Conceptos básicos de escalabilidad horizontal de un solo nodo](media/hana-vm-operations/scale-out-networking-overview.PNG)

Los límites admitidos hasta el momento son quince nodos de trabajo aparte un nodo maestro.

Desde el punto de vista del almacenamiento, la arquitectura de almacenamiento sería:


![Conceptos básicos de escalabilidad horizontal de un solo nodo](media/hana-vm-operations/scale-out-storage-overview.PNG)

El volumen **/hana/shared** se encuentra en la configuración del recurso compartido NFS de alta disponibilidad. Mientras tanto, todas las demás unidades se montan "localmente" en las máquinas virtuales individuales. 

### <a name="highly-available-nfs-share"></a>Recurso compartido NFS de alta disponibilidad
El clúster NFS de alta disponibilidad hasta el momento solo funciona con SUSE Linux. En el documento [Alta disponibilidad para NFS en máquinas virtuales de Azure en SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) se explica cómo configurarlo. Si no comparte el clúster NFS con cualquier otra configuración de HANA fuera de Azure Virtual Network, donde se ejecutan las instancias de SAP HANA, instálelo en la misma red virtual. Instálelo en su propia subred y asegúrese de que no todo el tráfico arbitrario puede tener acceso a la subred. En su lugar, conviene que limite el tráfico a esa subred para las direcciones IP de la máquina virtual que ejecutan el tráfico al volumen **/hana/shared**.

En relación con los VNIC de una máquina virtual de escalabilidad horizontal de HANA que debería enrutar el tráfico de **/hana/shared**, las recomendaciones son:

- Como el tráfico a **/hana/shared** es moderado, enrútelo a través del VNIC asignado a la red de cliente en la configuración mínima.
- En última instancia, para el tráfico a **/hana/shared**, implemente una subred de terceros en la red virtual que implemente la configuración de escalabilidad horizontal de SAP HANA y asigne un tercer VNIC que se hospede en esa subred. Utilice el tercer VNIC y la dirección IP asociada para el tráfico al recurso compartido NFS. Después puede aplicar reglas de enrutamiento y de acceso independientes.

>[!IMPORTANT]
>El tráfico de red entre las máquinas virtuales que tienen implementada la escalabilidad horizontal de SAP HANA y el recurso NFS de alta disponibilidad no puede enrutarse en ningún caso mediante [aplicaciones virtuales de red](https://azure.microsoft.com/solutions/network-appliances/) o dispositivos virtuales similares. Por otra parte, los grupos de seguridad de red de Azure no son tales dispositivos. Compruebe las reglas de enrutamiento para asegurarse de que las aplicaciones virtuales de red o dispositivos virtuales similares se desvían al acceder al recurso compartido NFS de alta disponibilidad desde las máquinas virtuales que ejecutan SAP HANA.
> 

Si desea compartir el clúster NFS de alta disponibilidad entre las configuraciones de SAP HANA, mueva todas esas configuraciones de HANA a la misma red virtual. 
 

### <a name="installing-sap-hana-scale-out-n-azure"></a>Instalación de la escalabilidad horizontal de SAP HANA en Azure
Para instalar una configuración de SAP para escalabilidad horizontal, debe seguir estos pasos:

- Implementar una nueva infraestructura de Azure Virtual Network o adaptarla.
- Implementación de las nuevas máquinas virtuales con volúmenes administrados de Azure Premium Storage
- Implementar un nuevo clúster NFS de alta disponibilidad o adoptar uno existente.
- Adaptar el enrutamiento de red para asegurarse de que, por ejemplo, la comunicación entre nodos de las máquinas virtuales no se enruta mediante una [NVA](https://azure.microsoft.com/solutions/network-appliances/). Sucede lo mismo para el tráfico entre las máquinas virtuales y el clúster NFS de alta disponibilidad.
- Instalar el nodo maestro de SAP HANA.
- Adaptar los parámetros de configuración del nodo maestro de SAP HANA.
- Continuar con la instalación de los nodos de trabajo de SAP HANA.

#### <a name="installation-of-sap-hana-in-scale-out-configuration"></a>Instalación de SAP HANA en la configuración de escalabilidad horizontal
Como se implementa la infraestructura de máquina virtual de Azure y se realizan todos los demás preparativos, deberá instalar las configuraciones de escalabilidad horizontal de SAP HANA en estos pasos:

- Instale el nodo maestro de SAP HANA según la documentación de SAP.
- **Después de la instalación, deberá cambiar el archivo global.ini y agregar el parámetro "basepath_shared = no" al archivo global.ini**. Este parámetro habilitará SAP HANA para que ejecute la escalabilidad horizontal sin volúmenes **/hana/data** y **/hana/log** "compartidos" entre los nodos. Los detalles están documentados en la [nota de SAP n.º 2080991](https://launchpad.support.sap.com/#/notes/2080991).
- Después de cambiar el parámetro global.ini, reinicie la instancia de SAP HANA.
- Agregue nodos de trabajo adicionales. Consulte también <https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US/0d9fe701e2214e98ad4f8721f6558c34.html>. Especifique la red interna para la comunicación entre nodos de SAP HANA durante la instalación o después con, por ejemplo, el hdblcm local. Para obtener documentación más detallada, vea también la [nota de SAP n.º 2183363](https://launchpad.support.sap.com/#/notes/2183363). 

Después de esta rutina de configuración, la configuración de escalabilidad horizontal instalada usará discos no compartidos para ejecutar **/hana/data** y **/hana/log**. El volumen **/hana/shared** se colocará en el recurso compartido NFS de alta disponibilidad.


## <a name="sap-hana-dynamic-tiering-20-for-azure-virtual-machines"></a>Dynamic Tiering 2.0 de SAP HANA para máquinas virtuales de Azure

Además de las certificaciones de SAP HANA en máquinas virtuales de la serie M de Azure, Dynamic Tiering 2.0 de SAP HANA también se admite en Microsoft Azure (consulte el vínculo de la documentación de Dynamic Tiering de SAP HANA, más abajo). Aunque no hay ninguna diferencia en la instalación del producto o en su funcionamiento, por ejemplo, a través de SAP HANA Cockpit dentro de una máquina virtual de Azure, hay algunos elementos importantes que son obligatorios para el soporte técnico oficial en Azure. A continuación se describen estos puntos clave. En este artículo, se usará la abreviatura "DT 2.0" en lugar del nombre completo Dynamic Tiering 2.0.

Dynamic Tiering 2.0 de SAP HANA no se admite en SAP BW ni en S4HANA. Los casos de uso principales son ahora las aplicaciones nativas de HANA.


### <a name="overview"></a>Información general

La ilustración siguiente proporciona información general sobre la compatibilidad de DT 2.0 en Microsoft Azure. Hay un conjunto de requisitos obligatorios, que deben seguirse para cumplir con la certificación oficial:

- DT 2.0 debe instalarse en una máquina virtual de Azure dedicada. No se puede ejecutar en la misma máquina virtual donde se ejecuta SAP HANA.
- Las máquinas virtuales de DT 2.0 y de SAP HANA deben implementarse dentro de la misma red virtual de Azure.
- Las máquinas virtuales de DT 2.0 y de SAP HANA deben implementarse con las redes aceleradas de Azure habilitadas.
- El tipo de almacenamiento para las máquinas virtuales de DT 2.0 debe ser Azure Premium Storage.
- Deben conectarse varios discos de Azure a la máquina virtual de DT 2.0.
- Es necesario crear una matriz redundante de discos independientes de software o volumen seccionado (ya sea a través de lvm o de mdadm) mediante la creación de bandas en los discos de Azure.

Esto se explica con más detalle en las secciones siguientes.

![Introducción a la arquitectura de DT 2.0 de SAP HANA](media/hana-vm-operations/hana-dt-20.PNG)



### <a name="dedicated-azure-vm-for-sap-hana-dt-20"></a>Máquina virtual de Azure dedicada para DT 2.0 de SAP HANA

En Azure IaaS, DT 2.0 solo se admite en una máquina virtual dedicada. No se permite ejecutar DT 2.0 en la misma máquina virtual de Azure donde se está ejecutando la instancia de HANA. Inicialmente, se pueden usar dos tipos de máquina virtual para ejecutar DT 2.0 de SAP HANA:

- M64-32ms 
- E32sv3 

Vea la descripción del tipo de máquina virtual [aquí](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory).

Dada la idea básica de DT 2.0, que es la descarga de datos "semiactivos" con el fin de ahorrar costos, tiene sentido usar los tamaños de máquina virtual correspondientes. Sin embargo, no hay ninguna regla estricta referente a las combinaciones posibles. Depende de la carga de trabajo específica del cliente.

Las configuraciones recomendadas serían:

| Tipo de máquina virtual de SAP HANA | Tipo de máquina virtual de DT 2.0 |
| --- | --- | 
| M128ms | M64-32ms |
| M128s | M64-32ms |
| M64ms | E32sv3 |
| M64s | E32sv3 |


Son posibles todas las combinaciones de máquinas virtuales de la serie M certificadas para SAP HANA con máquinas virtuales admitidas de DT 2.0 (M64-32ms, E32sv3).


### <a name="azure-networking-and-sap-hana-dt-20"></a>Redes de Azure y DT 2.0 de SAP HANA

Instalar DT 2.0 en una máquina virtual dedicada requiere un rendimiento de la red entre las máquinas virtuales de DT 2.0 y de SAP HANA de 10 Gb, como mínimo. Por lo tanto, es imprescindible colocar todas las máquinas virtuales dentro de la misma red virtual de Azure y habilitar las redes aceleradas de Azure.

Consulte información adicional sobre las redes aceleradas de Azure [aquí](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli).

### <a name="vm-storage-for-sap-hana-dt-20"></a>Almacenamiento de máquinas virtuales para DT 2.0 de SAP HANA

Según las instrucciones de procedimientos recomendados de DT 2.0, el rendimiento de E/S de disco debe ser de 50 MB/s por núcleo físico, como mínimo. Al observar la especificación de los dos tipos de máquina virtual de Azure, que son compatibles con DT 2.0, el límite máximo de rendimiento de E/S de disco para la máquina virtual es:

- E32sv3:   768 MB/s (no almacenado en caché), lo que significa una proporción de 48 MB/s por núcleo físico
- M64-32 ms:  1000 MB/s (no almacenado en caché), lo que significa una proporción de 62,5 MB/s por núcleo físico

Es necesario conectar varios discos de Azure a la máquina virtual de DT 2.0 y crear una matriz redundante de discos independientes de software (seccionado) en el nivel del sistema operativo para lograr el límite máximo de rendimiento de disco por máquina virtual. Un único disco de Azure no puede proporcionar el rendimiento para alcanzar el límite máximo para las máquinas virtuales en este sentido. Se requiere Azure Premium Storage para ejecutar DT 2.0. 

- [Aquí](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage) puede encontrar detalles sobre los tipos de disco de Azure disponibles.
- [Aquí](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid) puede encontrar detalles sobre la creación de una matriz redundante de discos independientes de software a través de mdadm.
- Los detalles sobre la configuración de LVM para crear un volumen seccionado con el máximo rendimiento se encuentran [aquí](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm).

Según los requisitos de tamaño, existen diferentes opciones para alcanzar el máximo rendimiento de una máquina virtual. Estas son las configuraciones de disco para volúmenes de datos posibles para cada tipo de máquina virtual de DT 2.0 a fin de alcanzar el límite superior de rendimiento para las máquinas virtuales. La máquina virtual E32sv3 debe considerarse como un nivel de entrada para las cargas de trabajo menores. En caso de que resultara no ser suficientemente rápida, podría ser necesario cambiar el tamaño de la máquina virtual a M64-32ms estándar.
Como la máquina virtual M64-32ms estándar tiene mucha memoria, la carga de E/S podría no alcanzar el límite, especialmente para cargas de trabajo con muchas lecturas. Por lo tanto, podrían ser suficientes menos discos en el conjunto seccionado, según la carga de trabajo específica del cliente. Sin embargo, por seguridad, se eligieron las configuraciones de disco siguientes a fin de garantizar el máximo rendimiento:


| SKU de la máquina virtual | Configuración de disco 1 | Configuración de disco 2 | Configuración de disco 3 | Configuración de disco 4 | Configuración de disco 5 | 
| ---- | ---- | ---- | ---- | ---- | ---- | 
| M64-32ms | 4 x P50 -> 16 TB | 4 x P40 -> 8 TB | 5 x P30 -> 5 TB | 7 x P20 -> 3,5 TB | 8 x P15 -> 2 TB | 
| E32sv3 | 3 x P50 -> 12 TB | 3 x P40 -> 6 TB | 4 x P30 -> 4 TB | 5 x P20 -> 2,5 TB | 6 x P15 -> 1,5 TB | 


Especialmente en el caso de que la carga de trabajo suponga muchas lecturas, podría aumentar el rendimiento de la E/S si se activa la memoria caché de host de Azure "de solo lectura", como se recomienda para los volúmenes de datos de software de base de datos. Sin embargo, la memoria caché del disco host de Azure del registro de transacciones debe ser "none" (ninguna). 

En relación con el tamaño del volumen de registro, el punto de partida recomendado es el valor heurístico del 15 % del tamaño de datos. La creación del volumen de registro puede llevarse a cabo con diferentes tipos de disco de Azure, según los requisitos de costo y rendimiento. Para el volumen de registro, se requiere una capacidad de proceso de E/S alto.  En el caso de usar el tipo de máquina virtual M64-32ms, se recomienda encarecidamente habilitar el [Acelerador de escritura](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator). Este Acelerador de escritura de Azure proporciona la latencia de escritura en disco óptima para el registro de transacciones (solo disponible para la serie M). Sin embargo, hay algunos elementos que deben tenerse en cuenta, como el número máximo de discos por tipo de máquina virtual. Puede encontrar detalles sobre el Acelerador de escritura [aquí](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator).


Estos son algunos ejemplos sobre cómo cambiar el tamaño del volumen de registro:

| tipo de disco y el tamaño del volumen de datos | configuración de tipo de disco y volumen de registro 1 | configuración de tipo de disco y volumen de registro 2 |
| --- | --- | --- |
| 4 x P50 -> 16 TB | 5 x P20 -> 2,5 TB | 3 x P30 -> 3 TB |
| 6 x P15 -> 1,5 TB | 4 x P6 -> 256 GB | 1 x P15 -> 256 GB |


Al igual que para el escalado horizontal de SAP HANA, el directorio /hana/shared tiene que compartirse entre la máquina virtual de SAP HANA y la máquina virtual de DT 2.0. Se recomienda la misma arquitectura que para la escalabilidad horizontal de SAP HANA con las máquinas virtuales, que actúan como un servidor NFS de alta disponibilidad. Con el fin de proporcionar un volumen compartido de copia de seguridad, se puede usar un diseño idéntico. Pero es decisión del cliente si se requiere una elevada disponibilidad o si basta con usar una máquina virtual dedicada con suficiente capacidad de almacenamiento para que actúe como un servidor de copia de seguridad.



### <a name="links-to-dt-20-documentation"></a>Vínculos a la documentación de DT 2.0 

- [SAP HANA Dynamic Tiering installation and update guide](https://help.sap.com/viewer/88f82e0d010e4da1bc8963f18346f46e/2.0.03/en-US) (Guía de instalación y actualización de Dynamic Tiering de SAP HANA)
- [SAP HANA Dynamic Tiering tutorials and resources](https://www.sap.com/developer/topics/hana-dynamic-tiering.html) (Tutoriales y recursos de Dynamic Tiering de SAP HANA)
- [SAP HANA Dynamic Tiering PoC](https://blogs.sap.com/2017/12/08/sap-hana-dynamic-tiering-delivering-on-low-tco-with-impressive-performance/) (Poc de Dynamic Tiering de SAP HANA)
- [SAP HANA 2.0 SPS 02 dynamic tiering enhancements](https://blogs.sap.com/2017/07/31/sap-hana-2.0-sps-02-dynamic-tiering-enhancements/) (Mejoras de Dynamic Tiering de SAP HANA 2.0 SPS 02)




## <a name="operations-for-deploying-sap-hana-on-azure-vms"></a>Operaciones de implementación de SAP HANA en máquinas virtuales de Azure
En las siguientes secciones se describen algunas de las operaciones relacionadas con la implementación de sistemas SAP HANA en máquinas virtuales de Azure.

### <a name="back-up-and-restore-operations-on-azure-vms"></a>Operaciones de copia de seguridad y restauración en máquinas virtuales Azure
En los documentos siguientes se describe cómo realizar la copia de seguridad y la restauración de una implementación de SAP HANA:

- [SAP HANA: información general sobre copias de seguridad](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [Azure Backup de SAP HANA en el nivel de archivo](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [Copia de seguridad de SAP HANA basada en instantáneas de almacenamiento](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)


### <a name="start-and-restart-vms-that-contain-sap-hana"></a>Iniciar y reiniciar máquinas virtuales que contienen SAP HANA
Una característica fundamental de la nube pública de Azure es que solo se le cobra por los minutos de computación. Por ejemplo, cuando se apaga una máquina virtual que ejecuta SAP HANA, se le facturan solo los costos de almacenamiento durante ese tiempo. Otra característica está disponible cuando se especifican las direcciones IP estáticas de las máquinas virtuales de la implementación inicial. Cuando se reinicia una máquina virtual con SAP HANA, lo hace con sus direcciones IP anteriores. 


### <a name="use-saprouter-for-sap-remote-support"></a>Usar SAPRouter para la compatibilidad remota de SAP
Si tiene una conexión de sitio a sitio entre sus ubicaciones locales y Azure, y ejecuta componentes de SAP, es probable que ya está ejecutando SAProuter. En este caso, realice las siguientes acciones para obtener compatibilidad remota:

- Mantener la dirección IP estática y privada de la máquina virtual que hospeda SAP HANA en la configuración de SAProuter.
- Configurar el NSG de la subred que hospeda la máquina virtual de HANA para permitir el tráfico a través del puerto TCP/IP 3299.

Si se está conectando a Azure a través de Internet y no tiene un enrutador SAP para la máquina virtual con SAP HANA, tiene que instalar el componente. Instale SAProuter en una máquina virtual independiente en la subred de administración. La imagen siguiente muestra un esquema aproximado de implementación de SAP HANA sin una conexión de sitio a sitio y con SAProuter:

![Esquema de implementación aproximado para SAP HANA sin conexión de sitio a sitio y SAPRouter](media/hana-vm-operations/hana-simple-networking3.PNG)

Asegúrese de instalar SAPRouter en otra máquina virtual y no en la máquina virtual de JumpBox. La máquina virtual independiente debe tener una dirección IP estática. Para conectar la instancia de SAProuter a la instancia de SAProuter hospedada en SAP, póngase en contacto con SAP para obtener una dirección IP. (La instancia de SAProuter hospedada en SAP es el equivalente de la instancia de SAProuter que se instala en la máquina virtual). Utilice la dirección IP de SAP para configurar la instancia de SAProuter. En la configuración, el único puerto necesario es el puerto TCP 3299.

Para más información sobre cómo configurar y mantener conexiones de compatibilidad remota a través de SAPRouter, consulte la [documentación de SAP](https://support.sap.com/en/tools/connectivity-tools/remote-support.html).

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>Alta disponibilidad con SAP HANA en máquinas virtuales nativas de Azure
Si ejecuta SUSE Linux Enterprise Server para aplicaciones SAP 12 SP1 o una versión posterior, puede establecer un clúster Pacemaker con dispositivos STONITH. Puede usar los dispositivos para establecer una configuración de SAP HANA que utilice la replicación sincrónica con conmutación por error automática y replicación del sistema de HANA. Para más información acerca del procedimiento de configuración, consulte [Alta disponibilidad de SAP HANA para máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).
