---
title: Servicios de almacenamiento de Azure Alemania | Microsoft Docs
description: Proporciona una comparación de los servicios de almacenamiento de Azure Alemania.
services: germany
cloud: na
documentationcenter: na
author: gitralf
manager: rainerst
ms.assetid: na
ms.service: germany
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/07/2017
ms.author: ralfwi
ms.openlocfilehash: fc3eaa92e2373384b59c65aae3d9d2d622a89e6f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67080841"
---
# <a name="azure-germany-storage-services"></a>Servicios de almacenamiento de Azure Alemania
## <a name="storage"></a>Storage
Para más información sobre Azure Storage y cómo usarlo, consulte la [documentación global de Azure Storage](../storage/index.yml).

Los datos almacenados en Azure Storage se replican para garantizar una alta disponibilidad. Para el almacenamiento con redundancia geográfica y el almacenamiento con redundancia geográfica con acceso de lectura, Azure replica datos entre *regiones emparejadas*. En el caso de Azure Alemania, estas regiones emparejadas son:

| Región primaria | Región secundaria (emparejamiento) |
| --- | --- |
| Centro de Alemania | Noreste de Alemania |
| Noreste de Alemania | Centro de Alemania |

La replicación de los datos conserva estos dentro de las fronteras alemanas. Las regiones principales y secundarias se emparejan para garantizar la distancia necesaria entre los centros de datos, con el fin de garantizar la disponibilidad en caso de una interrupción o un desastre en un área entera. Para el almacenamiento con redundancia geográfica de alta disponibilidad, seleccione un almacenamiento con redundancia geográfica o un almacenamiento con redundancia geográfica con acceso de lectura al crear una cuenta de almacenamiento.  

Storage Service Encryption protege los datos en reposo dentro de las cuentas de Azure Storage. Cuando se habilita esta característica, Azure cifra automáticamente los datos antes de continuar con el almacenamiento. Para ello, se usa el cifrado AES de 256 bits. Storage Service Encryption admite el cifrado de blobs en bloques, blobs en anexos y blobs en páginas.

### <a name="storage-service-availability-by-azure-germany-region"></a>Disponibilidad del servicio de almacenamiento por región de Azure Alemania

| Servicio | Centro de Alemania | Noreste de Alemania |
| --- | --- | --- |
| [Blob Storage](../storage/common/storage-introduction.md#blob-storage) |GA |GA |
| [Archivos de Azure](../storage/common/storage-introduction.md#azure-files) | GA | GA |
| [Table storage](../storage/common/storage-introduction.md#table-storage) |GA  |GA |
| [Queue Storage](../storage/common/storage-introduction.md#queue-storage) |GA | GA |
| [Niveles de Blob Storage de acceso frecuente y de acceso esporádico](../storage/blobs/storage-blob-storage-tiers.md) |GA |GA |
| [Cifrado del servicio de almacenamiento](../storage/common/storage-service-encryption.md) |GA |GA |
| Import/Export |N/D |N/D |
| StorSimple |N/D |N/D |

### <a name="variations"></a>Variaciones
Las direcciones URL de las cuentas de almacenamiento de Azure Alemania son diferentes de las de Azure global:

| Tipo de servicio | Azure global | Azure Alemania |
| --- | --- | --- |
| Almacenamiento de blobs | \* .blob.core.windows.net | *.blob.core.cloudapi.de |
| Archivos de Azure | *.file.core.windows.net | *.file.core.cloudapi.de | 
| Queue Storage | *.queue.core.windows.net | *.queue.core.cloudapi.de |
| Almacenamiento de tablas | *.table.core.windows.net | *.table.core.cloudapi.de |

> [!NOTE]
> Todos los scrips y todo el código deben tener en cuenta los puntos de conexión adecuados. Para obtener más información sobre las cadenas de conexión, consulte [Configuración de las cadenas de conexión de Azure Storage](../storage/common/storage-configure-connection-string.md). 
>
>

Para más información sobre las API, consulte el [constructor de cuenta de almacenamiento de nube](/dotnet/api/microsoft.azure.cosmos.table.cloudstorageaccount.-ctor).

El sufijo del punto de conexión que se usará en estas sobrecargas es *core.cloudapi.de*.

> [!NOTE]
> Si aparece el error 53 ("No se ha encontrado la ruta de acceso de la red") mientras [monta el recurso compartido de archivos](../storage/files/storage-dotnet-how-to-use-files.md), un firewall podría bloquear el puerto de salida. Intentar montar el recurso compartido de archivos en la máquina virtual que se encuentre en la misma suscripción de Azure que la cuenta de almacenamiento.
>
>


## <a name="next-steps"></a>Pasos siguientes
Para información complementaria y actualizaciones, suscríbase al [blog de Azure Alemania](https://blogs.msdn.microsoft.com/azuregermany/).
