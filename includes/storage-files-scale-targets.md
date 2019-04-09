---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 99314538f90404d7c2b72da0dd2da2d8ac60a08a
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59291851"
---
| Recurso | Recursos compartidos de archivos estándar | Recursos compartidos de archivos Premium (versión preliminar) |
|----------|---------------|------------------------------------------|
| Tamaño máximo de un recurso compartido de archivos | No hay ningún mínimo; pago por uso | 100 GB; aprovisionado |
| Tamaño máximo de un recurso compartido de archivos | 5 TiB | (Versión preliminar pública) de 5 TB, 100 TB (versión preliminar pública limitada) |
| Tamaño máximo de un archivo en un recurso compartido de archivos | 1 TiB | 1 TiB |
| Número máximo de archivos en un recurso compartido de archivos | Sin límite | Sin límite |
| Número máximo de IOPS por recurso compartido | 1000 E/S POR SEGUNDO | 5.120 IOPS bases con el límite de ráfaga 15.360 (versión preliminar), 100 000 e/s por segundo (versión preliminar pública limitada)|
| Número máximo de directivas de acceso almacenadas por cada archivo de recurso compartido | 5 | 5 |
| Rendimiento de un único recurso compartido de destino | Hasta 60 MiB/s | Vea los valores de entrada y salida de la cuota del archivo premium|
| Salida máxima para un recurso compartido de archivo único | Ver el rendimiento de destino de recurso compartido de archivos estándar | Hasta 368 MiB/s (versión preliminar), hasta 6,204 MiB/s (versión preliminar pública limitada) |
| Entrada máxima para un recurso compartido de archivo único | Ver el rendimiento de destino de recurso compartido de archivos estándar | Hasta 245 MiB/s (versión preliminar), hasta 4.136 MiB/s (versión preliminar pública limitada) |
| Número máximo de identificadores abiertos por archivo | 2.000 identificadores abiertos | 2.000 identificadores abiertos |
| Número máximo de instantáneas de recurso compartido | 200 instantáneas de recurso compartido | 200 instantáneas de recurso compartido |
| Longitud máxima del nombre de objeto (archivos y directorios) | 2.048 caracteres | 2.048 caracteres |
| Número máximo de componentes de la ruta de acceso (en la ruta de acceso \A\B\C\D, cada letra es un componente) | 255 caracteres | 255 caracteres |