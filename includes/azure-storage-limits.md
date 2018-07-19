---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 04/03/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 6572adb0d8d629910492603a17988b89acce2f17
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38756036"
---
| Recurso | Límite predeterminado |
| --- | --- |
| Número de cuentas de almacenamiento por suscripción y región | 200<sup>1</sup> |
| Capacidad máxima de la cuenta de almacenamiento | 500 TiB<sup>2</sup> |
| Número máximo de contenedores de blobs, blobs, recursos compartidos de archivos, tablas, colas, entidades o mensajes por cuenta de almacenamiento | Sin límite |
| Tasa de solicitud total por cuenta de almacenamiento | 20 000 solicitudes por segundo<sup>2</sup> |
| Entrada máxima<sup>3</sup> por cuenta de almacenamiento (regiones de EE. UU.) | 10 Gbps si RA-GRS/GRS están habilitados, 20 Gbps en el caso de LRS/ZRS<sup>4</sup> |
| Salida máxima<sup>3</sup> por cuenta de almacenamiento (regiones de EE. UU.) | 20 Gbps si RA-GRS/GRS están habilitados, 30 Gbps en el caso de LRS/ZRS<sup>4</sup> |
| Entrada máxima<sup>3</sup> por cuenta de almacenamiento (regiones no de EE. UU.) | 5 Gbps si RA-GRS/GRS están habilitados, 10 Gbps en el caso de LRS/ZRS<sup>4</sup> |
| Salida máxima<sup>3</sup> por cuenta de almacenamiento (regiones no de EE. UU.) | 10 Gbps si RA-GRS/GRS están habilitados, 15 Gbps en el caso de LRS/ZRS<sup>4</sup> |

<sup>1</sup>Incluye las cuentas de almacenamiento Estándar y Premium. Si necesita más de 200 cuentas de almacenamiento en una región dada, realice una solicitud a través del [servicio de soporte técnico de Azure](https://azure.microsoft.com/support/faq/). El equipo de Azure Storage revisará su caso empresarial y podría aprobar hasta 250 cuentas de almacenamiento para una región dada. 

<sup>2</sup> Si necesita límites ampliados de su cuenta de almacenamiento, póngase en contacto con el [soporte técnico de Azure](https://azure.microsoft.com/support/faq/). El equipo de Azure Storage revisa la solicitud y puede aprobar límites según el caso. Tanto las cuentas de uso general como las de Blob Storage admiten aumento de capacidad, entrada/salida y tasa de solicitudes por solicitud. Para información sobre los nuevos valores máximos para las cuentas de Blob Storage, consulte [Announcing larger, higher scale storage accounts](https://azure.microsoft.com/blog/announcing-larger-higher-scale-storage-accounts/) (Anuncio de cuentas de almacenamiento más grandes y a mayor escala).

<sup>3</sup> limitado únicamente por los límites de entrada/salida de la cuenta. *Entrada* hace referencia a todos los datos (solicitudes) que se envían a una cuenta de almacenamiento. *Salida* hace referencia a todos los datos (respuestas) recibidos desde una cuenta de almacenamiento.  

<sup>4</sup>Las opciones de redundancia de Azure Storage incluyen:
* **RA-GRS**: almacenamiento con redundancia geográfica con acceso de lectura. Si RA-GRS está habilitada, los destinos de salida para la ubicación secundaria son idénticos a los de la ubicación principal.
* **GRS**: almacenamiento con redundancia geográfica. 
* **ZRS**: almacenamiento con redundancia de zona.
* **LRS**: almacenamiento con redundancia local. 
