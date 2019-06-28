---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/23/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: d96e69fb526cff633c78e9ac8a1679762014cd4b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67133701"
---
Azure Storage ofrece varios tipos de cuentas de almacenamiento. Cada tipo admite diferentes características y tiene su propio modelo de precios. Tenga en cuenta estas diferencias antes de crear una cuenta de almacenamiento para determinar el tipo de cuenta más adecuada para sus aplicaciones. Estos son los tipos de cuentas de almacenamiento:

- **Cuentas de uso general v2**: tipo de cuenta de almacenamiento básico para blobs, archivos, colas y tablas. Se recomienda para la mayoría de los escenarios con Azure Storage.
- **Cuentas de uso general v1**: tipo de cuenta heredada para blobs, archivos, colas y tablas. Cuando sea posible, utilice en su lugar las cuentas de uso general v2.
- **Cuentas de almacenamiento de blobs en bloques**: Cuentas de almacenamiento solo para blobs con características de rendimiento superiores. Recomendado para escenarios con altas tasas de transacciones, que utilizan objetos más pequeños o que requieren una latencia de almacenamiento constantemente baja.
- **Cuentas de almacenamiento FileStorage (versión preliminar)** : Cuentas de almacenamiento solo para archivos con características de rendimiento superiores. Se recomienda para empresas y aplicaciones de escalado de alto rendimiento.
- **Cuentas de almacenamiento de blobs**: cuentas de almacenamiento de solo blobs. Cuando sea posible, utilice en su lugar las cuentas de uso general v2.

En la siguiente tabla se describen los tipos de cuentas de almacenamiento y sus capacidades:

| Tipo de cuenta de almacenamiento | Servicios admitidos                       | Niveles de rendimiento admitidos      | Niveles de acceso admitidos         | Opciones de replicación               | Modelo de implementación<div role="complementary" aria-labelledby="deployment-model"><sup>1</sup></div> | Cifrado<div role="complementary" aria-labelledby="encryption"><sup>2</sup></div> |
|----------------------|------------------------------------------|-----------------------------|--------------------------------|-----------------------------------|------------------------------|------------------------|
| Uso general v2   | Blob, archivo, cola, tabla y disco       | Estándar, Premium<div role="complementary" aria-labelledby="premium-performance"><sup>5</sup></div> | Frecuente, esporádico, archivo<div role="complementary" aria-labelledby="archive"><sup>3</sup></div> | LRS, GRS, RA-GRS, ZRS<div role="complementary" aria-labelledby="zone-redundant-storage"><sup>4</sup></div> | Resource Manager             | Cifrados              |
| Uso general v1   | Blob, archivo, cola, tabla y disco       | Estándar, Premium<div role="complementary" aria-labelledby="premium-performance"><sup>5</sup></div> | N/D                            | LRS, GRS, RA-GRS                  | Resource Manager, clásico    | Cifrados              |
| Almacenamiento de blobs en bloque   | Blob (solo blobs en bloques y blobs en anexos) | Premium                       | N/D                            | LRS                               | Resource Manager             | Cifrados              |
| FileStorage (versión preliminar)   | Solo archivos | Premium                       | N/D                            | LRS                               | Resource Manager             | Cifrados              |
| Almacenamiento de blobs         | Blob (solo blobs en bloques y blobs en anexos) | Estándar                      | Frecuente, esporádico, archivo<div role="complementary" aria-labelledby="archive"><sup>3</sup></div> | LRS, GRS, RA-GRS                  | Resource Manager             | Cifrados              |

<div id="deployment-model"><sup>1</sup>Se recomienda usar el modelo de implementación de Azure Resource Manager. Las cuentas de almacenamiento que usan el modelo de implementación clásico aún se pueden crear en algunas ubicaciones; asimismo, se seguirán admitiendo las cuentas clásicas ya existentes. Para obtener más información, consulte <a href="https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-model">Implementación de Azure Resource Manager frente a la implementación clásica: Conozca los modelos de implementación y el estado de los recursos</a>.</div>

<div id="encryption"><sup>2</sup>Todas las cuentas de almacenamiento se encriptan mediante Storage Service Encryption (SSE) para los datos en reposo. Para más información, consulte <a href="https://docs.microsoft.com/azure/storage/common/storage-service-encryption">Cifrado del servicio Azure Storage para datos en reposo</a>.</div>

<div id="archive"><sup>3</sup>El nivel de archivo está disponible solo en el nivel de un blob individual, y no en el nivel de la cuenta de almacenamiento. Solo se pueden archivar los blobs en bloques y los blocs adjuntos. Para obtener más información, consulte <a href="https://docs.microsoft.com/azure/storage/blobs/storage-blob-storage-tiers">Azure Blob Storage: Niveles de almacenamiento de acceso frecuente, de acceso esporádico y de acceso de archivo</a>.</div>

<div id="zone-redundant-storage"><sup>4</sup>El almacenamiento con redundancia de zona (ZRS) solo está disponible para las cuentas de almacenamiento de uso general estándar v2. Para obtener más información sobre ZRS, consulte <a href="https://docs.microsoft.com/azure/storage/common/storage-redundancy-zrs">Almacenamiento con redundancia de zona (ZRS): aplicaciones de Azure Storage de alta disponibilidad</a>. Para obtener más información acerca de otras opciones de replicación, consulte <a href="https://docs.microsoft.com/azure/storage/common/storage-redundancy">Replicación de Azure Storage</a>.</div>

<div id="premium-performance"><sup>5</sup>El rendimiento prémium para las cuentas v2 y v1 de uso general está disponible solo para blobs en páginas y en disco.</div>
