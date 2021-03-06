---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 02/27/2020
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: e29cdd56d1c43b3d0e8fc6ca233ac19d8b0004ff
ms.sourcegitcommit: 021ccbbd42dea64d45d4129d70fff5148a1759fd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2020
ms.locfileid: "78357434"
---
En la siguiente tabla se describen los límites predeterminados para las cuentas de almacenamiento habilitadas de Azure de uso general v1 (GPv1), v2 (GPv2), Blob, Premium BlockBlob y Data Lake Gen2. El límite de *entrada* hace referencia a todos los datos que se envían a una cuenta de almacenamiento. El límite de *salida* hace referencia a todos los datos que se reciben de una cuenta de almacenamiento.

| Resource | Límite predeterminado |
| --- | --- |
| Número de cuentas de almacenamiento por región y suscripción, incluidas las cuentas Estándar, Premium y Data Lake Gen2<sup>3</sup> | 250 |
| Capacidad máxima de la cuenta de almacenamiento | 5 PiB <sup>1</sup>|
| Número máximo de contenedores de blobs, blobs, recursos compartidos de archivos, tablas, colas, entidades o mensajes por cuenta de almacenamiento | Sin límite |
| Tasa de solicitud total<sup>1</sup> por cuenta de almacenamiento | 20 000 solicitudes por segundo |
| Entrada máxima<sup>1</sup> por cuenta de almacenamiento (regiones de Estados Unidos y Europa) | 25 Gbps |
| Entrada máxima<sup>1</sup> por cuenta de almacenamiento (regiones distintas de Estados Unidos y Europa) | 5 Gbps si RA-GRS o GRS está habilitado, 10 Gbps en el caso de LRS o ZRS<sup>2</sup> |
| Salida máxima para uso general v2 y las cuentas de Blob Storage (todas las regiones) | 50 Gbps |
| Salida máxima para cuentas de almacenamiento v1 de uso general (regiones de EE. UU.) | 20 Gbps si RA-GRS o GRS están habilitado, 30 Gbps en el caso de LRS o ZRS<sup>2</sup> |
| Salida máxima para cuentas de almacenamiento v1 de uso general (regiones no de EE. UU.) | 10 Gbps si RA-GRS o GRS está habilitado, 15 Gbps en el caso de LRS o ZRS<sup>2</sup> |
| Número máximo de reglas de red virtual por cuenta de almacenamiento | 200 |
| Número máximo de reglas de dirección IP por cuenta de almacenamiento | 200 |

<sup>1</sup>Las cuentas estándar de Azure Storage admiten límites de capacidad y límites de entrada por solicitud superiores. Para solicitar un aumento en los límites de cuenta, póngase en contacto con el [soporte técnico de Azure](https://azure.microsoft.com/support/faq/).

<sup>2</sup> Si la cuenta de almacenamiento tiene habilitado el acceso de lectura con almacenamiento con redundancia geográfica (RA-GRS) o el almacenamiento con redundancia de zona geográfica (RA-GZRS), los destinos de salida de la ubicación secundaria son idénticos a los de la ubicación principal. Las opciones de [Replicación de Azure Storage ](https://docs.microsoft.com/azure/storage/common/storage-redundancy) incluyen:

[!INCLUDE [azure-storage-redundancy](azure-storage-redundancy.md)]

<sup>3</sup> [Azure Data Lake Storage Gen2](../articles/storage/blobs/data-lake-storage-introduction.md) es un conjunto de funcionalidades dedicadas al análisis de macrodatos basado en Azure Blob Storage. Las limitaciones de Azure Storage y del almacenamiento de blobs se aplican a Data Lake Gen2.

> [!NOTE]
> Microsoft recomienda usar una cuenta de almacenamiento de uso general v2 en la mayoría de los escenarios. Puede actualizar fácilmente una cuenta de Azure Blob Storage o de uso general v1 a una cuenta de uso general v2 sin tiempo de inactividad y sin la necesidad de copiar datos. Para más información, consulte [Actualización a una cuenta de almacenamiento de uso general v2](../articles/storage/common/storage-account-upgrade.md).

Si las necesidades de su aplicación superan los objetivos de escalabilidad de una sola cuenta de almacenamiento, puede compilar la aplicación de forma que use varias cuentas de almacenamiento. A continuación, puede dividir los objetos de datos en esas cuentas de almacenamiento. Para obtener información sobre los precios por volumen, vea [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

Todas las cuentas de almacenamiento se ejecutan en una topología de red plana, independientemente del momento en que se hayan creado. Para obtener más información acerca de la arquitectura de red plana de Azure Storage y de la escalabilidad, vea [Microsoft Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx) (Microsoft Azure Storage: Un servicio de almacenamiento en nube altamente disponible de gran coherencia). Se puede habilitar un [espacio de nombres jerárquico se puede habilitar para una cuenta de Data Lake Gen2](../articles/storage/blobs/data-lake-storage-namespace.md), además del espacio de nombres plano para el acceso multiprotocolo. Las cuentas de almacenamiento de espacios de nombres planos y jerárquicos admiten los mismos objetivos de escalabilidad y rendimiento que se describen en este artículo.
