---
title: Mover el almacenamiento de mainframe a Azure Storage
description: Recursos de Azure storage y muy escalable pueden ayudar a las organizaciones basadas en grandes sistemas migrar y modernizar aplicaciones z14 de IBM.
author: njray
ms.author: larryme
ms.date: 04/02/2019
ms.topic: article
ms.service: storage
ms.openlocfilehash: 4fea787e7fe20d60de91761811e1b69bebf3a010
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58896479"
---
# <a name="move-mainframe-storage-to-azure"></a>Mover el almacenamiento de mainframe a Azure

Para ejecutar cargas de trabajo de mainframes en Microsoft Azure, debe saber cómo se comparan las capacidades de su sistema central en Azure. Los recursos de almacenamiento masivo y escalable pueden ayudar a las organizaciones comienzan a modernizar sin abandonar las aplicaciones que dependen.

Azure proporciona características similares a mainframe y capacidad de almacenamiento que es comparable a sistemas basados en z14 de IBM (el modelo más reciente cuando se redactó este documento). Este artículo explica cómo obtener resultados comparables en Azure.

## <a name="mainframe-storage-at-a-glance"></a>Almacenamiento del sistema central con un solo vistazo

El gran sistema IBM caracteriza a almacenamiento de dos maneras. El primero es un dispositivo de almacenamiento de acceso directo (DASD). La segunda es almacenamiento secuencial. Para administrar el almacenamiento, el gran sistema proporciona datos Facility Storage Management Subsystem (DFSMS). Administra el acceso a datos para los distintos dispositivos de almacenamiento.

[DASD](https://en.wikipedia.org/wiki/Direct-access_storage_device) hace referencia a un dispositivo independiente para el almacenamiento secundario de (no en los memoria) que permite una dirección única que se usará para el acceso directo de datos. Originalmente, el término DASD aplicado a unidades de discos, tambores magnéticos o las celdas de datos. Sin embargo, ahora el término también se puede aplicar a dispositivos de almacenamiento de estado sólido (SSD), redes de área de almacenamiento (SAN), conectado a red (NAS) de almacenamiento y unidades de disco ópticas. Para los fines de este documento, DASD hace referencia a unidades de disco, SANs y SSD.

A diferencia del almacenamiento DASD, almacenamiento secuencial en un gran sistema hace referencia a dispositivos, como unidades de cinta que se obtiene acceso desde un punto de partida, a continuación, leen o escriben datos en una línea.

Dispositivos de almacenamiento están conectados normalmente mediante una conexión de fibra (FICON) o se accede directamente sobre el uso de bus de E/S del mainframe [HiperSockets](https://www.ibm.com/support/knowledgecenter/zosbasics/com.ibm.zos.znetwork/znetwork_85.htm), una tecnología de IBM para comunicaciones de alta velocidad entre las particiones en un servidor con una hipervisor.

La mayoría de los sistemas mainframe separan almacenamiento en dos tipos:

- *Almacenamiento en línea* (también conocido como almacenamiento de acceso frecuente) es necesario para las operaciones diarias. Almacenamiento DASD normalmente se usa para este propósito. Sin embargo, almacenamiento secuencial, como cinta copias de seguridad diarias (lógicos o físicos), también puede usarse para este propósito.

- *Almacenamiento de archivo* (también conocido como almacenamiento en frío) no se garantiza que se monte en un momento dado. En su lugar, se monta y acceso según sea necesario. Almacenamiento de archivo a menudo se implementa mediante las copias de seguridad de cinta secuencial (físicos o lógicos) para el almacenamiento.

## <a name="mainframe-versus-io-latency-and-iops"></a>Mainframe frente a la latencia de E/S y e/s por segundo

Mainframes a menudo se usan para las aplicaciones que requieren alto rendimiento de E/S y baja latencia de E/S. Para ello puede usar las conexiones FICON a dispositivos de E/S y HiperSockets. Cuando HiperSockets se usan para conectar dispositivos y aplicaciones directamente al canal de E/S de un gran sistema, se puede conseguir la latencia en los microsegundos.

## <a name="azure-storage-at-a-glance"></a>Almacenamiento de Azure con un solo vistazo

Azure infraestructura como-servicio ([IaaS](https://azure.microsoft.com/overview/what-is-iaas/)) opciones de almacenamiento de proporcionan capacidad de mainframe comparable.

Microsoft ofrece a los correspondientes a petabytes de almacenamiento para las aplicaciones hospedadas en Azure y tiene varias opciones de almacenamiento. Estas van desde el almacenamiento SSD de alto rendimiento para el almacenamiento de blobs de bajo costo para los archivos y almacenamiento masivo. Además, Azure proporciona una opción de redundancia de datos para el almacenamiento, algo que requiere más esfuerzo para configurar en un entorno de mainframes.

Almacenamiento de Azure está disponible como [Azure Disks](/azure/virtual-machines/windows/managed-disks-overview), [Azure Files](/azure/storage/files/storage-files-introduction), y [Blobs de Azure](/azure/storage/blobs/storage-blobs-overview) tal como se resume en la tabla siguiente. Obtenga más información sobre [cuándo usar cada](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks).

<!-- markdownlint-disable MD033 -->

<table>
<thead>
    <tr><th>Type</th><th>DESCRIPCIÓN</th><th>Use lo que quiera para:</th></tr>
</thead>
<tbody>
<tr><td>Archivos de Azure
</td>
<td>
Proporciona una interfaz SMB, bibliotecas de cliente y un <a href="https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api">REST</a> interfaz que permite el acceso desde cualquier lugar a archivos almacenados.
</td>
<td><ul>
<li>Levantar y mover una aplicación a la nube cuando la aplicación usa las API del sistema de archivos nativo para compartir datos entre ella y otras aplicaciones que se ejecutan en Azure.</li>
<li>Store de desarrollo y depuración de las herramientas que necesitan tener acceso desde muchas máquinas virtuales.</li>
</ul>
</td>
</tr>
<tr><td>Azure Blobs
</td>
<td>Proporciona bibliotecas de cliente y un <a href="https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api">REST</a> interfaz que permite que los datos no estructurados se almacenen y acceder a gran escala en blobs en bloques. También admite <a href="/azure/storage/blobs/data-lake-storage-introduction">Azure Data Lake Storage Gen2</a> para soluciones de análisis de macrodatos empresariales.
</td>
<td><ul>
<li>Admite escenarios de acceso aleatorio y streaming en una aplicación.</li>
<li>Tener acceso a datos de la aplicación desde cualquier lugar.</li>
<li>Cree un lago de datos empresariales en Azure y realizar análisis de macrodatos.</li>
</ul></td>
</tr>
<tr><td>Azure Disks
</td>
<td>Proporciona bibliotecas de cliente y un <a href="https://docs.microsoft.com/rest/api/compute/disks">REST</a> interfaz que permite que los datos se almacenen persistentemente y se obtiene acceso desde un disco duro virtual conectado.
</td>
<td><ul>
<li>Levantar y mover las aplicaciones que usan las API del sistema de archivos nativo para leer y escribir datos en discos persistentes.</li>
<li>Store los datos que no es necesarios para tener acceso desde fuera de la máquina virtual a la que está conectado el disco.</li>
</ul></td>
</tr>
</tbody>
</table>
<!-- markdownlint-enable MD033 -->

## <a name="azure-hot-online-and-cold-archive-storage"></a>Azure activa (en línea) y almacenamiento en frío (archivo)

El tipo de almacenamiento para un sistema determinado depende de los requisitos del sistema, incluidos el tamaño de almacenamiento, rendimiento e IOPS. Para el almacenamiento de DASD-type en un gran sistema, las aplicaciones en Azure normalmente usar almacenamiento en unidades de discos de Azure en su lugar. Para almacenamiento de archivo del sistema central, se usa el almacenamiento de blobs en Azure.

Las SSD ofrecen el máximo rendimiento de almacenamiento en Azure. Las siguientes opciones están disponibles (a partir de la escritura de este documento):

| Type         | Tamaño           | E/S                  |
|--------------|----------------|-----------------------|
| SSD ultra    | 4 GB hasta 64 TB  | IOPS de 1.200 a 160.000 |
| SSD Premium  | 32 GB a 32 TB | 12 a 15 000 e/s por segundo     |
| SSD estándar | 32 GB a 32 TB | IOPS de 12 a 2.000      |

Almacenamiento de blobs proporciona el mayor volumen de almacenamiento en Azure. Además de tamaño de almacenamiento, Azure ofrece almacenamiento administrado y no administrados. Con almacenamiento administrado, Azure se encarga de administrar las cuentas de almacenamiento subyacente. Con el almacenamiento no administrado, el usuario asume la responsabilidad de configurar las cuentas de almacenamiento de Azure del tamaño adecuado cumplir los requisitos de almacenamiento.

## <a name="next-steps"></a>Pasos siguientes

- [Migración del sistema central](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/overview)
- [Mainframe rehospedaje en Azure Virtual Machines](/azure/virtual-machines/workloads/mainframe-rehosting/overview)
- [Mover el proceso del sistema central a Azure](mainframe-compute-Azure.md)
- [Decisión sobre cuándo usar Azure Blobs, Azure Files o Azure Disks](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks)
- [SSD discos administrados estándar para las cargas de trabajo de máquina virtual de Azure](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd)

### <a name="ibm-resources"></a>Recursos de IBM

- [Sysplex paralela en IBM Z](https://www.ibm.com/it-infrastructure/z/technologies/parallel-sysplex-resources)
- [IBM CICS y las instalaciones de acoplamiento: Más allá de los conceptos básicos](http://www.redbooks.ibm.com/redbooks/pdfs/sg248420.pdf)
- [Creación de usuarios requeridos para una instalación de la característica de Db2 pureScale](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/t0055374.html?pos=2)
- [Db2icrt - crear comando de instancia](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.admin.cmd.doc/doc/r0002057.html)
- [PureScale Clustered solución de base de datos de Db2](http://www.ibmbigdatahub.com/blog/db2-purescale-clustered-database-solution-part-1)
- [Datos de IBM Studio](https://www.ibm.com/developerworks/downloads/im/data/index.html/)

### <a name="azure-government"></a>Azure Government

- [En la nube de Microsoft Azure Government para aplicaciones del sistema central](https://azure.microsoft.com/resources/microsoft-azure-government-cloud-for-mainframe-applications/)
- [Microsoft y FedRAMP](https://www.microsoft.com/TrustCenter/Compliance/FedRAMP)

### <a name="more-migration-resources"></a>Más recursos de migración

- [Alianza de modernización de plataforma: IBM Db2 en Azure](https://www.platformmodernization.org/pages/ibmdb2azure.aspx)
- [Centro de datos Virtual de Azure Lift- and -Shift guía](https://azure.microsoft.com/resources/azure-virtual-datacenter-lift-and-shift-guide/)
- [GlusterFS iSCSI](https://docs.gluster.org/en/latest/Administrator%20Guide/GlusterFS%20iSCSI/)
