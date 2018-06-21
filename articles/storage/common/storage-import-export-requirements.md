---
title: Requisitos del servicio Azure Import/Export | Microsoft Docs
description: Conozca los requisitos de hardware y software del servicio Azure Import/Export.
author: alkohli
manager: jeconnoc
services: storage
ms.service: storage
ms.topic: article
ms.date: 06/06/2018
ms.author: alkohli
ms.openlocfilehash: 4c6e22f50f4550cb4a6e25960bcc13a4d92e9819
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2018
ms.locfileid: "34825074"
---
# <a name="azure-importexport-system-requirements"></a>Requisitos del sistema de Azure Import/Export

En este artículo se describen los requisitos importantes del servicio Azure Import/Export. Se recoienda que revise cuidadosamente la información antes de usar el servicio Import/Export y que luego vuelva a ella si es necesario durante el funcionamiento.

## <a name="supported-operating-systems"></a>Sistemas operativos compatibles

Para preparar las unidades de disco duro con la herramienta WAImportExport, se admite el siguiente **sistema operativo de 64 bits que admite Cifrado de unidad BitLocker**.


|Plataforma |Versión |
|---------|---------|
|Windows     | Windows 7 Enterprise, Windows 7 Ultimate <br> Windows 8 Pro, Windows 8 Enterprise, Windows 8.1 Pro, Windows 8.1 Enterprise <br> Windows 10        |
|Windows Server     |Windows Server 2008 R2 <br> Windows Server 2012, Windows Server 2012 R2         |



## <a name="supported-storage-accounts"></a>Cuentas de almacenamiento admitidas

El servicio Azure Import/Export admite las siguientes cuentas de almacenamiento de Azure.
- Clásico
- Cuentas de Blob Storage
- Cuentas de almacenamiento de uso general v1 

Puede utilizar cada trabajo para transferir datos desde o hacia una sola cuenta de almacenamiento. Dicho de otra forma, un trabajo de importación y exportación no puede abarcar varias cuentas de almacenamiento. Para obtener información acerca de la creación de una nueva cuenta de almacenamiento, consulte [Creación de una cuenta de almacenamiento](storage-create-storage-account.md#create-a-storage-account).

> [!IMPORTANT] 
> El servicio Azure Import/Export no admite cuentas de almacenamiento con la característica [Puntos de conexión del servicio Virtual Network](../../virtual-network/virtual-network-service-endpoints-overview.md) habilitada. 

## <a name="supported-storage-types"></a>Tipos de almacenamiento admitidos

La siguiente lista muestra los tipos de almacenamiento que se admiten con el servicio Azure Import/Export.


|Trabajo  |Storage  |Compatible  |No compatible  |
|---------|---------|---------|---------|
|Importar     |  Azure Blob Storage. <br>Blobs en bloques y blobs en páginas admitidos. <br> Archivos de Azure admitidos.       |         |
|Exportación     |   Azure Blob Storage. <br>Blobs en bloques, blobs en páginas y blobs en anexos admitidos.       | Archivos de Azure no admitidos.        |


## <a name="supported-hardware"></a>Hardware admitido 

Para el servicio Azure Import/Export, se necesitan discos admitidos y conectores SATA admitidos para copiar los datos.

### <a name="supported-disks"></a>Discos admitidos

En la siguiente lista se muestran los discos que se pueden usar con el servicio Import/Export.


|Tipo de disco  |Tamaño  |Compatible |No compatible  |
|---------|---------|---------|---------|
|SSD    |   2,5"      |         |         |
|HDD     |  2,5"<br>3,5"       |SATA II, SATA III         |Unidad de disco duro externa con adaptador USB integrado <br> Disco dentro de la carcasa de una unidad de disco duro externa         |


Un único trabajo de importación o exportación puede tener:
- 10 HDD o SSD como máximo.
- Una combinación de HDD y SSD de cualquier tamaño.

Un volumen grande de unidades puede distribuirse entre varios trabajos, y no hay ningún límite en el número de trabajos que se pueden crear. 

Para los trabajos de importación, solo se procesa el primer volumen de datos de la unidad. El volumen de datos debe tener formato NTFS.

### <a name="supported-external-usb-adaptors"></a>Adaptadores USB externos admitidos

Al preparar las unidades de disco duro y copiar los datos mediante la herramienta WAImportExport, puede usar los siguientes adaptadores de USB externos (listos para usar): 
- Anker 68UPSATAA-02BU
- Anker 68UPSHHDS-BU
- Startech SATADOCK22UE
- Orico 6628SUS3-C-BK (serie 6628)
- Estación de acoplamiento de unidades de disco duro externas SATA intercambiables en caliente Thermaltake BlacX (USB 2.0 y eSATA)


## <a name="next-steps"></a>Pasos siguientes

* [Configuración de la herramienta WAImportExport](storage-import-export-tool-how-to.md)
* [Introducción a la utilidad de línea de comandos AzCopy](storage-use-azcopy.md)
* [Ejemplo de API de REST de Azure Import/Export](https://azure.microsoft.com/documentation/samples/storage-dotnet-import-export-job-management/)

