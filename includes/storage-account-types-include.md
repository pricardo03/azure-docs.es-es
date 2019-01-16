---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 01/02/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: b278cce8f885f31f9d59fd389261812e04e58405
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/07/2019
ms.locfileid: "54066997"
---
Azure Storage ofrece tres tipos de cuentas de almacenamiento. Cada tipo admite diferentes características y tiene su propio modelo de precios. Tenga en cuenta estas diferencias antes de crear una cuenta de almacenamiento para determinar el tipo de cuenta más adecuada para sus aplicaciones. Estos son los tres tipos de cuentas de almacenamiento:

* **Cuentas de uso general v2**: tipo de cuenta de almacenamiento básico para blobs, archivos, colas y tablas. Se recomienda para la mayoría de los escenarios con Azure Storage.
* **Cuentas de uso general v1**: tipo de cuenta heredada para blobs, archivos, colas y tablas. Cuando sea posible, utilice en su lugar las cuentas de uso general v2.
* **Cuentas de almacenamiento de blobs**: cuentas de almacenamiento de solo blobs. Cuando sea posible, utilice en su lugar las cuentas de uso general v2. 

En la siguiente tabla se describen los tipos de cuentas de almacenamiento y sus capacidades:

| Tipo de cuenta de almacenamiento | Servicios admitidos                       | Niveles de rendimiento admitidos | Niveles de acceso admitidos               | Opciones de replicación                                                | Modelo de implementación<sup>1</sup>  | Cifrado<sup>2</sup> |
|----------------------|------------------------------------------|-----------------------------|--------------------------------------|--------------------------------------------------------------------|-------------------|------------|
| Uso general v2   | Blob, archivo, cola, tabla y disco       | Estándar, Premium           | Frecuente, Esporádico, Archivo<sup>3</sup> | LRS, ZRS<sup>4</sup>, GRS, RA-GRS | Resource Manager | Cifrados  |
| Uso general v1   | Blob, archivo, cola, tabla y disco       | Estándar, Premium           | N/D                                  | LRS, GRS, RA-GRS                                                   | Resource Manager, clásico  | Cifrados  |
| Almacenamiento de blobs         | Blob (solo blobs en bloques y blobs en anexos) | Estándar                    | Frecuente, Esporádico, Archivo<sup>3</sup>                            | LRS, GRS, RA-GRS                                                   | Resource Manager  | Cifrados  |

<sup>1</sup>Se recomienda usar el modelo de implementación de Azure Resource Manager. Las cuentas de almacenamiento que usan el modelo de implementación clásico aún se pueden crear en algunas ubicaciones; asimismo, se seguirán admitiendo las cuentas clásicas ya existentes. Para obtener más información, consulte [Implementación de Azure Resource Manager frente a la implementación clásica: Conozca los modelos de implementación y el estado de los recursos](../articles/azure-resource-manager/resource-manager-deployment-model.md).

<sup>2</sup>Todas las cuentas de almacenamiento se encriptan mediante Storage Service Encryption (SSE) para los datos en reposo. Para más información, consulte [Cifrado del servicio Azure Storage para datos en reposo](../articles/storage/common/storage-service-encryption.md).

<sup>3</sup>El nivel de archivo está disponible solo en el nivel de un blob individual, y no en el nivel de la cuenta de almacenamiento. Solo se pueden archivar los blobs en bloques y los blocs adjuntos. Para obtener más información, consulte [Azure Blob Storage: Niveles de almacenamiento de acceso frecuente, de acceso esporádico y de acceso de archivo](../articles/storage/blobs/storage-blob-storage-tiers.md).

<sup>4</sup>El almacenamiento con redundancia de zona (ZRS) solo está disponible para las cuentas de almacenamiento de uso general estándar v2. Para obtener más información sobre ZRS, consulte [Almacenamiento con redundancia de zona (ZRS): aplicaciones de Azure Storage de alta disponibilidad](../articles/storage/common/storage-redundancy-zrs.md). Para obtener más información acerca de otras opciones de replicación, consulte [Replicación de Azure Storage](../articles/storage/common/storage-redundancy.md).
