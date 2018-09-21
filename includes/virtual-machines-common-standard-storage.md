---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: yuemlu
ms.service: storage
ms.topic: include
ms.date: 06/05/2018
ms.author: yuemlu
ms.custom: include file
ms.openlocfilehash: ab085d6a5cb38c46cf46a51da6d294732e2fd879
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2018
ms.locfileid: "45979729"
---
# <a name="cost-effective-standard-storage-and-unmanaged-and-managed-azure-vm-disks"></a>Standard Storage rentable y discos de máquina virtual de Azure administrados y no administrados

Azure Standard Storage ofrece compatibilidad de discos confiable y de bajo coste para las máquinas virtuales que ejecutan cargas de trabajo que no tienen en cuenta la latencia. También admite blobs, tablas, colas y archivos. Con Standard Storage, los datos se almacenan en unidades de disco duro (HDD). Cuando se trabaja con máquinas virtuales, se pueden usar discos SSD y HDD de almacenamiento estándar para escenarios de desarrollo/pruebas y para cargas de trabajo menos críticas, y discos SSD premium para aplicaciones de producción críticas. Standard Storage está disponible en todas las regiones de Azure. 

Este artículo se centra en el uso de discos SSD y HDD estándar. Para más información acerca del uso de Storage con blobs, tablas, colas y archivos, consulte [Introducción a Microsoft Azure Storage](../articles/storage/common/storage-introduction.md).

## <a name="disk-types"></a>Tipos de disco

Hay dos maneras de crear discos estándar para máquinas virtuales de Azure:

**Discos no administrados**: este es el método original, en el que se administran las cuentas de almacenamiento que se utilizan para almacenar los archivos VHD que se corresponden con los discos de máquina virtual. Los archivos VHD se almacenan como blobs en páginas en las cuentas de almacenamiento. Los discos no administrados se pueden conectar a máquina virtual de Azure de cualquier tamaño, incluidas aquellas que usan principalmente Premium Storage, como las series DSv2 y GS. Las VM de Azure admiten la conexión de varios discos estándar, lo que permite un máximo de 256 TB de almacenamiento por máquina virtual.

[**Azure Managed Disks**](../articles/virtual-machines/windows/managed-disks-overview.md): esta característica administra las cuentas de almacenamiento que se usan automáticamente para los discos de máquina virtual. Especifique el tipo (SSD premium, SSD estándar o HDD estándar) y el tamaño del disco que necesita, y Azure lo crea y administra automáticamente. No tiene que preocuparse de colocar los discos en varias cuentas de almacenamiento para estar seguro de que permanece dentro de los límites de escalabilidad de las cuentas de almacenamiento: Azure se encarga de ello.

Aunque están disponibles los dos tipos de discos, se recomienda utilizar Managed Disks para aprovechar su gran cantidad de características.

Para empezar a usar Azure Standard Storage, visite la página de [creación de una cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/). 

Para obtener información acerca de cómo crear una máquina virtual con Managed Disks, consulte uno de los artículos siguientes.

* [Creación de una máquina virtual con Resource Manager y PowerShell](../articles/virtual-machines/windows/quick-create-powershell.md)
* [Creación de una máquina virtual con Linux desde cero con la CLI de Azure 2.0](../articles/virtual-machines/linux/quick-create-cli.md)

## <a name="standard-storage-features"></a>Características de Standard Storage 

Examinemos algunas de las características de Standard Storage. Para más información, consulte [Introducción a Azure Storage](../articles/storage/common/storage-introduction.md).

**Standard Storage**: Azure Standard Storage admite discos de Azure, blobs de Azure, archivos de Azure, tablas de Azure y colas de Azure. Para utilizar el almacenamiento estándar, comience por [crear una cuenta de Azure Storage](../articles/storage/common/storage-quickstart-create-account.md).

**Discos SSD estándar:** estos proporcionan un rendimiento más fiable que los discos HDD estándar y están disponibles actualmente en versión preliminar. Para más información acerca de la disponibilidad en regiones de los discos SSD estándar, consulte la [disponibilidad en regiones de los discos SSD estándar (versión preliminar)](../articles/virtual-machines/windows/faq-for-disks.md#standard-ssds-azure-regions).

**Discos HDD estándar:** los discos HDD estándar se pueden conectar a todas las máquinas virtuales de Azure, incluidas las máquinas virtuales de varias series de tamaños que se usan con Premium Storage, como las series DSv2 y GS. Un disco HDD estándar solo puede conectarse a una máquina virtual. Sin embargo, se pueden asociar uno o varios de estos discos a una máquina virtual, hasta el número máximo definido para el tamaño de la máquina virtual. En la sección siguiente sobre objetivos de escalabilidad y rendimiento de Standard Storage, se describen las especificaciones con más detalle.

**Blob en páginas estándar**: los blobs en páginas estándar se utilizan para contener discos persistentes para las máquinas virtuales y, al igual que otros tipos de blobs de Azure, se puede acceder a ellos directamente a través de REST. Los [blob en páginas](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs) son una colección de páginas de 512 bytes optimizadas para operaciones de lectura y escritura aleatorias. 

**Replicación del almacenamiento:** en la mayoría de las regiones, los datos de una cuenta de almacenamiento estándar se pueden replicar localmente o replicar geográficamente en varios centros de datos. Los cuatro tipos de replicación disponible son: el almacenamiento con redundancia local (LRS), el almacenamiento con redundancia de zona (ZRS), el almacenamiento con redundancia geográfica (GRS) y el almacenamiento con redundancia geográfica con acceso de lectura (RA-GRS). Actualmente, Managed Disks de Standard Storage solo admiten el almacenamiento con redundancia local (LRS). Para más información, consulte [Replicación de Azure Storage](../articles/storage/common/storage-redundancy.md).

## <a name="scalability-and-performance-targets"></a>Objetivos de escalabilidad y rendimiento

En esta sección, se describen los objetivos de escalabilidad y rendimiento que se deben tener en cuenta al usar el almacenamiento estándar.

### <a name="account-limits--does-not-apply-to-managed-disks"></a>Límites de cuenta (no se aplica a discos administrados)

| **Recurso** | **Límite predeterminado** |
|--------------|-------------------|
| TB por cuenta de almacenamiento  | 500 TB |
| Entrada máxima<sup>1</sup> por cuenta de almacenamiento (regiones de EE. UU.) | 10 Gbps si GRS/ZRS están habilitados, 20 Gbps en el caso de LRS |
| Salida máxima<sup>1</sup> por cuenta de almacenamiento (regiones de EE. UU.) | 20 Gbps si RA-GRS/GRS/ZRS están habilitados, 30 Gbps en el caso de LRS |
| Entrada máxima<sup>1</sup> por cuenta de almacenamiento (regiones de Europa y Asia) | 5 Gbps si GRS/ZRS están habilitados, 10 Gbps en el caso de LRS |
| Salida máxima<sup>1</sup> por cuenta de almacenamiento (regiones de Europa y Asia) | 10 Gbps si RA-GRS/GRS/ZRS están habilitados, 15 Gbps en el caso de LRS |
| Tasa total de solicitudes (suponiendo un tamaño de objeto de 1 KB) por cuenta de almacenamiento | Hasta 20.000 IOPS, entidades por segundo o mensajes por segundo |

<sup>1</sup>El término "entrada" hace referencia todos los datos (solicitudes) que se envían a una cuenta de almacenamiento. El término "salida" hace referencia a todos los datos (respuestas) que se reciben de una cuenta de almacenamiento.

Para obtener más información, consulte [Objetivos de escalabilidad y rendimiento de Azure Storage](../articles/storage/common/storage-scalability-targets.md).

Si las necesidades de su aplicación superan los objetivos de escalabilidad de una cuenta de almacenamiento individual, compile la aplicación de modo que use varias cuentas de almacenamiento y divida los datos entre ellas. Como alternativa, puede usar Azure Managed Disks, en cuyo caso Azure administra automáticamente la creación de particiones y la colocación de los datos.

### <a name="standard-disks-limits"></a>Límites de los discos estándar

A diferencia de los discos Premium, las operaciones de entrada/salida por segundo (IOPS) y el rendimiento (ancho de banda) de los discos estándar no se aprovisionan. El rendimiento de los discos estándar varía en función del tamaño de la máquina virtual a la que está conectada el disco, no del tamaño del disco. Lo normal es lograr, como máximo, el límite de rendimiento de la tabla siguiente.

**Límites de los discos estándar (administrados y no administrados)**

| **Nivel de máquina virtual**            | **Máquina virtual de nivel Básico** | **Máquina virtual de nivel Estándar** |
|------------------------|-------------------|----------------------|
| Tamaño máx. de disco          | 4095 GB           | 4095 GB              |
| Máximo de 8 KB de IOPS por disco | Hasta 300         | Hasta 500            |
| Ancho de banda máx. por disco | Hasta 60 MB/s     | Hasta 60 MB/s        |

Si la carga de trabajo requiere compatibilidad con discos de gran rendimiento y baja latencia, considere la posibilidad de usar Premium Storage. Para conocer más ventajas de Premium Storage, vaya a [Premium Storage: almacenamiento de alto rendimiento para cargas de trabajo de máquina virtual de Azure](../articles/virtual-machines/windows/premium-storage.md). 

## <a name="snapshots-and-copy-blob"></a>Instantáneas y copia de blob

Para el servicio Storage, el archivo VHD es un blob en páginas. Es posible tomar instantáneas de blobs en páginas y copiarlos en otra ubicación, como por ejemplo en otra cuenta de almacenamiento.

### <a name="unmanaged-disks"></a>Discos no administrados

Se pueden crear [instantáneas incrementales](../articles/virtual-machines/windows/incremental-snapshots.md) para discos estándar no administrados de la misma manera que se usan las instantáneas con Standard Storage. Si el disco de origen se encuentra en una cuenta de almacenamiento con redundancia local, se recomienda crear instantáneas y, luego, copiarlas en una cuenta de almacenamiento estándar con redundancia geográfica. Para obtener más información, consulte [Opciones de redundancia de Azure Storage](../articles/storage/common/storage-redundancy.md).

Si un disco está conectado a una máquina virtual, determinadas operaciones de la API no se permiten en los discos. Por ejemplo, no puede realizar una operación [Copy Blob](/rest/api/storageservices/Copy-Blob) en ese blob mientras el disco esté conectado a una máquina virtual. En su lugar, cree una instantánea del blob mediante el método de API de REST [Snapshot Blob](/rest/api/storageservices/Snapshot-Blob) y, luego, realice la operación [Copy Blob](/rest/api/storageservices/Copy-Blob) de la instantánea para copiar el disco conectado. Como alternativa, puede separar el disco y, después, realizar las operaciones necesarias.

Para mantener copias con redundancia geográfica de las instantáneas, puede copiar dichas instantáneas de una cuenta de almacenamiento con redundancia local a una cuenta de almacenamiento estándar con redundancia geográfica mediante AzCopy o Copy Blob. Para más información, consulte [Transferencia de datos con la utilidad en línea de comandos AzCopy](../articles/storage/common/storage-use-azcopy.md) y [Copy Blob](/rest/api/storageservices/Copy-Blob).

Para más información acerca de cómo realizar operaciones de REST en blobs en páginas en cuentas de almacenamiento estándar, consulte [Azure Storage Services REST API Reference](/rest/api/storageservices/Azure-Storage-Services-REST-API-Reference) (Referencia de API de RES de los servicios de Azure Storage).

### <a name="managed-disks"></a>Discos administrados

Una instantánea administrada es una copia de solo lectura de un disco administrado que se almacena como disco administrado estándar. Actualmente, las instantáneas incrementales no son compatibles con Managed Disks, pero lo serán en el futuro.

Si un disco administrado está conectado a una máquina virtual, determinadas operaciones de la API no se permiten en los discos. Por ejemplo, mientras el disco esté conectado a una máquina virtual, no se puede generar una firma de acceso compartido (SAS) para realizar una operación de copia. En su lugar, primero hay que crear una instantánea del disco y, después, realizar la copia de la instantánea. Como alternativa, se puede separar el disco y, luego, generar una firma de acceso compartido (SAS) para realizar la operación de copia.

## <a name="pricing-and-billing"></a>Precios y facturación

Al usar Standard Storage, se aplican las siguientes consideraciones de facturación:

* Tamaño de datos o datos no administrados del almacenamiento estándar 
* Discos administrados estándar
* Instantáneas del almacenamiento estándar
* Transferencias de datos de salida
* Transacciones

**Tamaño de disco y datos de almacenamiento no administrado:** en el caso de discos no administrados y otros datos (blobs, tablas, colas y archivos), se cobra solo para la cantidad de espacio que se usa. Por ejemplo, si tiene una máquina virtual cuyo blob en páginas se aprovisiona como 127 GB, pero en realidad solo usa 10 GB de espacio, eso es lo que se le va a facturar. Se admite almacenamiento Estándar de hasta 8191 GB y discos no administrados estándar de hasta 4095 GB. 

**Discos administrados:** la facturación de los discos administrados estándar depende del tamaño aprovisionado del disco. Azure asigna el tamaño aprovisionado (redondeado) a la opción de disco de Managed Disks más cercana, como se especifica en las tablas siguientes. Cada disco administrado se asigna a uno de los tamaños aprovisionados admitidos y se factura según corresponda. Por ejemplo, si crea un disco administrado estándar y especifica un tamaño aprovisionado de 200 GiB, se le facturará según los precios del tipo de disco S15.

| **Tipo de disco<br> administrado HDD estándar** | **S4** | **S6** | **S10** | **S15** | **S20** | **S30** | **S40** | **S50** |
|------------------|---------|---------|--------|--------|--------|----------------|----------------|----------------| 
| Tamaño del disco        | 32 GiB  | 64 GiB  | 128 GB | 256 GiB | 512 GB | 1024 GiB (1 TiB) | 2048 GiB (2 TiB) | 4095 GiB (4 TiB) | 


**Instantáneas**: las instantáneas de los discos estándar se facturan por la capacidad adicional que usan. Para obtener información sobre las instantáneas, consulte [Crear una instantánea de un blob](/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob).

**Transferencias de datos de salida**: las [transferencias de datos de salida](https://azure.microsoft.com/pricing/details/data-transfers/) (datos que salen de los centros de datos de Azure) se facturan en función del uso de ancho de banda.

**Transacción**: Azure cobra 0,0036 $ por 100 000 transacciones de almacenamiento estándar. Dichas transacciones incluyen las operaciones de lectura y escritura en el almacenamiento.

Para más información acerca de los precios de Standard Storage, Virtual Machines y Managed Disks, consulte:

* [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
* [Precios de máquinas virtuales](https://azure.microsoft.com/pricing/details/virtual-machines/)
* [Precios de Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks)

## <a name="azure-backup-service-support"></a>Soporte técnico del servicio Azure Backup 

Se pueden realizar copias de seguridad de máquinas virtuales con discos no administrados mediante Azure Backup. [Más detalles](../articles/backup/backup-azure-vms-first-look-arm.md).

El servicio Azure Backup también se puede usar con Managed Disks para crear un trabajo de copia de seguridad con copias de seguridad basadas en tiempo, fácil restauración de la máquina virtual y directivas de retención de copia de seguridad. En [Uso de máquinas virtuales de disco administrado con Azure Backup](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup) puede encontrar más información al respecto.

## <a name="next-steps"></a>Pasos siguientes

* [Introducción a Azure Storage](../articles/storage/common/storage-introduction.md)

* [crear una cuenta de almacenamiento](../articles/storage/common/storage-quickstart-create-account.md)

* [Introducción a Managed Disks](../articles/virtual-machines/linux/managed-disks-overview.md)

* [Creación de una máquina virtual con Resource Manager y PowerShell](../articles/virtual-machines/windows/quick-create-powershell.md)

* [Creación de una máquina virtual con Linux desde cero con la CLI de Azure 2.0](../articles/virtual-machines/linux/quick-create-cli.md)
