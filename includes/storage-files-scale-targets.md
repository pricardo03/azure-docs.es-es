---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: ec75cf35bb503e49885664a6682e3be319cc45f6
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57554068"
---
| Recurso | Recursos compartidos de archivos estándar | Recursos compartidos de archivos Premium (versión preliminar) |
|----------|---------------|------------------------------------------|
| Tamaño máximo de un recurso compartido de archivos | No hay ningún mínimo; pago por uso | 100 GiB |
| Tamaño máximo de un recurso compartido de archivos | 5 TiB | 5 TiB |
| Tamaño máximo de un archivo en un recurso compartido de archivos | 1 TiB | 1 TiB |
| Número máximo de archivos en un recurso compartido de archivos | Sin límite | Sin límite |
| Número máximo de IOPS por recurso compartido | 1000 E/S POR SEGUNDO | Línea de base de e/s por segundo 5.120<br />15.360 IOPS con ráfaga |
| Número máximo de directivas de acceso almacenadas por cada archivo de recurso compartido | 5 | 5 |
| Rendimiento de un único recurso compartido de destino | Hasta 60 MiB/s | Hasta 612 MiB/segundo (aprovisionado) |
| Número máximo de identificadores abiertos por archivo | 2.000 identificadores abiertos | 2.000 identificadores abiertos |
| Número máximo de instantáneas de recurso compartido | 200 instantáneas de recurso compartido | 200 instantáneas de recurso compartido |
| Longitud máxima del nombre de objeto (archivos y directorios) | 2.048 caracteres | 2.048 caracteres |
| Número máximo de componentes de la ruta de acceso (en la ruta de acceso \A\B\C\D, cada letra es un componente) | 255 caracteres | 255 caracteres |