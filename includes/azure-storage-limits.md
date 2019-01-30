---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 01/11/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: e4fa42b6c32c3eb383eea4489ea109c0d496bdb9
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2019
ms.locfileid: "54392694"
---
En la tabla siguiente se describen los límites predeterminados para Azure Storage. El límite de *entrada* hace referencia a todos los datos (solicitudes) que se envían a una cuenta de almacenamiento. El límite de *salida* hace referencia a todos los datos (respuestas) recibidos desde una cuenta de almacenamiento.

| Recurso | Límite predeterminado |
| --- | --- |
| Número de cuentas de almacenamiento por región y suscripción, incluidas las cuentas Estándar y Premium | 250 |
| Capacidad máxima de la cuenta de almacenamiento | 2 PB para Estados Unidos y Europa; 500 TB para el resto de regiones, incluido el Reino Unido |
| Número máximo de contenedores de blobs, blobs, recursos compartidos de archivos, tablas, colas, entidades o mensajes por cuenta de almacenamiento | Sin límite |
| Tasa de solicitud total<sup>1</sup> por cuenta de almacenamiento | 20 000 solicitudes por segundo |
| Entrada máxima<sup>1</sup> por cuenta de almacenamiento (regiones de EE. UU.) | 10 Gbps si RA-GRS o GRS están habilitados, 20 Gbps en el caso de LRS o ZRS<sup>2</sup> |
| Entrada máxima<sup>1</sup> por cuenta de almacenamiento (regiones no de EE. UU.) | 5 Gbps si RA-GRS o GRS están habilitados, 10 Gbps en el caso de LRS o ZRS<sup>2</sup> |
| Salida máxima para uso general v2 y las cuentas de almacenamiento de blobs (todas las regiones) | 50 Gbps |
| Salida máxima para cuentas de almacenamiento v1 de uso general (regiones de EE. UU.) | 20 Gbps si RA-GRS o GRS están habilitados; 30 Gbps en el caso de LRS o ZRS <sup>2</sup> |
| Salida máxima para cuentas de almacenamiento v1 de uso general (regiones no de EE. UU.) | 10 Gbps si RA-GRS o GRS están habilitados; 15 Gbps en el caso de LRS o ZRS <sup>2</sup> |

<sup>1</sup> Las cuentas de almacenamiento estándar de Azure admiten límites más altos de entrada por solicitud. Para solicitar un aumento en los límites de cuenta para entrada, póngase en contacto con el [soporte técnico de Azure](https://azure.microsoft.com/support/faq/).

<sup>2</sup> [Las opciones de replicación de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-redundancy) incluyen:
* **RA-GRS**: almacenamiento con redundancia geográfica con acceso de lectura. Si RA-GRS está habilitada, los destinos de salida para la ubicación secundaria son idénticos a los de la ubicación principal.
* **GRS**: almacenamiento con redundancia geográfica. 
* **ZRS**: almacenamiento con redundancia de zona.
* **LRS**: Almacenamiento con redundancia local. 

> [!NOTE]
> Microsoft recomienda usar cuentas de almacenamiento de uso general v2 en la mayoría de los escenarios. Puede actualizar fácilmente una cuenta de Blob Storage o de uso general v1 a una cuenta de uso general v2 sin tiempo de inactividad y sin la necesidad de copiar datos.
>
> Para más información sobre las cuentas de Azure Storage, vea [Introducción a la cuenta de Azure Storage](../articles/storage/common/storage-account-overview.md). 

Si las necesidades de su aplicación superan los objetivos de escalabilidad de una sola cuenta de almacenamiento, puede compilar la aplicación de forma que use varias cuentas de almacenamiento. A continuación, puede dividir los objetos de datos en esas cuentas de almacenamiento. Para obtener información sobre los precios por volumen, consulte [Precios de Azure Storage](https://azure.microsoft.com/pricing/details/storage/) .

Todas las cuentas de almacenamiento se ejecutan en la topología de red plana y admiten los objetivos de escalabilidad y rendimiento descritos en este artículo, independientemente de su frecha de creación. Para obtener más información acerca de la arquitectura de red plana de Azure Storage y de la escalabilidad, vea [Microsoft Azure Storage: A Highly Available Cloud Storage Service with Strong Consistency](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx) (Microsoft Azure Storage: Un servicio de almacenamiento en nube altamente disponible de gran coherencia).

