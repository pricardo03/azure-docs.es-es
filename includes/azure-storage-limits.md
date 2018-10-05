---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/25/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: a0a777151216cb70b696da088b8a0d34779ebc39
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47396134"
---
En la tabla siguiente se describen los límites predeterminados para Azure Storage. El límite de *entrada* hace referencia a todos los datos (solicitudes) que se envían a una cuenta de almacenamiento. El límite de *salida* hace referencia a todos los datos (respuestas) recibidos desde una cuenta de almacenamiento.

| Recurso | Límite predeterminado |
| --- | --- |
| Número de cuentas de almacenamiento por región y suscripción, incluidas las cuentas Estándar y Premium | 200 |
| Capacidad máxima de la cuenta de almacenamiento<sup>1</sup> | 500 TiB |
| Número máximo de contenedores de blobs, blobs, recursos compartidos de archivos, tablas, colas, entidades o mensajes por cuenta de almacenamiento | Sin límite |
| Tasa de solicitud total<sup>1</sup> por cuenta de almacenamiento | 20 000 solicitudes por segundo |
| Entrada máxima<sup>1</sup> por cuenta de almacenamiento (regiones de EE. UU.) | 10 Gbps si RA-GRS o GRS están habilitados, 20 Gbps en el caso de LRS o ZRS<sup>2</sup> |
| Salida máxima<sup>1</sup> por cuenta de almacenamiento (regiones de EE. UU.) | 20 Gbps si RA-GRS o GRS están habilitados, 30 Gbps en el caso de LRS o ZRS |
| Entrada máxima<sup>1</sup> por cuenta de almacenamiento (regiones no de EE. UU.) | 5 Gbps si RA-GRS o GRS están habilitados, 10 Gbps en el caso de LRS o ZRS<sup>2</sup> |
| Salida máxima<sup>1</sup> por cuenta de almacenamiento (regiones no de EE. UU.) | 10 Gbps si RA-GRS o GRS están habilitados, 15 Gbps en el caso de LRS o ZRS |

<sup>1</sup> Las cuentas de almacenamiento de Azure admiten límites más altos de capacidad, tasa de solicitud, entrada y salida por solicitud. Para más información sobre los límites aumentados, consulte [Announcing larger, higher scale storage accounts](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/) (Anuncio de cuentas de almacenamiento más grandes y con mayor escala). Para solicitar un aumento en los límites de cuenta, póngase en contacto con el [soporte técnico de Azure](https://azure.microsoft.com/support/faq/).

Las opciones de <sup>2</sup>[replicación de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-redundancy) incluyen:
* **RA-GRS**: almacenamiento con redundancia geográfica con acceso de lectura. Si RA-GRS está habilitada, los destinos de salida para la ubicación secundaria son idénticos a los de la ubicación principal.
* **GRS**: almacenamiento con redundancia geográfica. 
* **ZRS**: almacenamiento con redundancia de zona.
* **LRS**: almacenamiento con redundancia local. 

