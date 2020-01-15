---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 12/13/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: a0ecb4f5fe54e9895898156893c1e686c6cc24e6
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75392272"
---
En la tabla siguiente se describen los límites predeterminados para las cuentas de Blob Storage y Azure v1 y v2 de uso general. El *límite de entrada* hace referencia a todos los datos de las solicitudes que se envían a una cuenta de almacenamiento. El límite de *salida* hace referencia a todos los datos de las respuestas que se reciben desde una cuenta de almacenamiento.

| Resource | Límite predeterminado |
| --- | --- |
| Número de cuentas de almacenamiento por región y suscripción, incluidas las cuentas Estándar y Premium | 250 |
| Capacidad máxima de la cuenta de almacenamiento | 2 PiB para Estados Unidos y Europa; 500 TiB para el resto de regiones (incluido el Reino Unido)<sup>1</sup>|
| Número máximo de contenedores de blobs, blobs, recursos compartidos de archivos, tablas, colas, entidades o mensajes por cuenta de almacenamiento | Sin límite |
| Tasa de solicitud total<sup>1</sup> por cuenta de almacenamiento | 20 000 solicitudes por segundo |
| Entrada máxima<sup>1</sup> por cuenta de almacenamiento (regiones de Estados Unidos y Europa) | 25 Gbps |
| Entrada máxima<sup>1</sup> por cuenta de almacenamiento (regiones distintas de Estados Unidos y Europa) | 5 Gbps si RA-GRS o GRS está habilitado, 10 Gbps en el caso de LRS o ZRS<sup>2</sup> |
| Salida máxima para uso general v2 y las cuentas de Blob Storage (todas las regiones) | 50 Gbps |
| Salida máxima para cuentas de almacenamiento v1 de uso general (regiones de EE. UU.) | 20 Gbps si RA-GRS o GRS están habilitado, 30 Gbps en el caso de LRS o ZRS<sup>2</sup> |
| Salida máxima para cuentas de almacenamiento v1 de uso general (regiones no de EE. UU.) | 10 Gbps si RA-GRS o GRS está habilitado, 15 Gbps en el caso de LRS o ZRS<sup>2</sup> |
| Número máximo de reglas de red virtual por cuenta de almacenamiento | 200 |
| Número máximo de reglas de dirección IP por cuenta de almacenamiento | 200 |

<sup>1</sup>Las cuentas estándar de Azure Storage admiten límites de capacidad y límites de entrada por solicitud superiores. Para solicitar un aumento en los límites de cuenta para entrada, póngase en contacto con el [soporte técnico de Azure](https://azure.microsoft.com/support/faq/). Para más información, consulte [Announcing larger, higher scale storage accounts](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/) (Anuncio de cuentas de almacenamiento más grandes y con mayor escala).

<sup>2</sup> Si la cuenta de almacenamiento tiene habilitado el acceso de lectura con almacenamiento con redundancia geográfica (RA-GRS) o el almacenamiento con redundancia de zona geográfica (RA-GZRS), los destinos de salida de la ubicación secundaria son idénticos a los de la ubicación principal. Las opciones de [Replicación de Azure Storage ](https://docs.microsoft.com/azure/storage/common/storage-redundancy) incluyen:

[!INCLUDE [azure-storage-redundancy](azure-storage-redundancy.md)]

> [!NOTE]
> Microsoft recomienda usar una cuenta de almacenamiento de uso general v2 en la mayoría de los escenarios. Puede actualizar fácilmente una cuenta de Azure Blob Storage o de uso general v1 a una cuenta de uso general v2 sin tiempo de inactividad y sin la necesidad de copiar datos. Para más información, consulte [Actualización a una cuenta de almacenamiento de uso general v2](../articles/storage/common/storage-account-upgrade.md).

Si las necesidades de su aplicación superan los objetivos de escalabilidad de una sola cuenta de almacenamiento, puede compilar la aplicación de forma que use varias cuentas de almacenamiento. A continuación, puede dividir los objetos de datos en esas cuentas de almacenamiento. Para obtener información sobre los precios por volumen, vea [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

Todas las cuentas de almacenamiento se ejecutan en la topología de red plana y admiten los objetivos de escalabilidad y rendimiento descritos en este artículo, independientemente de su frecha de creación. Para obtener más información acerca de la arquitectura de red plana de Azure Storage y de la escalabilidad, vea [Microsoft Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx) (Microsoft Azure Storage: Un servicio de almacenamiento en nube altamente disponible de gran coherencia).
