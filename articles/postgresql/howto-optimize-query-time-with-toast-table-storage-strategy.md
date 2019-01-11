---
title: Optimización del tiempo de consulta en un servidor de Azure Database for PostgreSQL mediante la estrategia de almacenamiento de tablas TOAST
description: En este artículo se describe cómo optimizar el tiempo de consulta con la estrategia de almacenamiento de tablas TOAST en un servidor de Azure Database for PostgreSQL.
author: dianaputnam
ms.author: dianas
ms.service: postgresql
ms.topic: conceptual
ms.date: 10/22/2018
ms.openlocfilehash: 1fb818a65e26f969f72131b0f5265f3efdd36bb6
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2018
ms.locfileid: "53542227"
---
# <a name="optimizing-query-time-with-toast-table-storage-strategy"></a>Optimización del tiempo de consulta con la estrategia de almacenamiento de tablas TOAST 
En este artículo se describe cómo optimizar los tiempos de consulta con la estrategia de almacenamiento de tablas TOAST.

## <a name="toast-table-storage-strategies"></a>Estrategias de almacenamiento de tablas TOAST
Hay cuatro estrategias distintas para almacenar columnas con capacidad para TOAST en el disco que representan diversas combinaciones entre la compresión y el almacenamiento fuera de línea. La estrategia puede establecerse en el nivel de tipo de datos y el nivel de columna.
- **Plain** evita la compresión o el almacenamiento fuera de línea; además, deshabilita el uso de encabezados de byte único para los tipos de varlena. **Plain** es la única estrategia posible para las columnas de tipos de datos sin capacidad TOAST.
- **Extended** permite tanto la compresión como el almacenamiento fuera de línea. **Extended** es el valor predeterminado para la mayoría de los tipos de datos con capacidad TOAST. Primero se intentará la compresión y, después, el almacenamiento fuera de línea si la fila sigue siendo demasiado grande.
- **External** permite el almacenamiento fuera de línea, pero no la compresión. El uso de **External** acelerará las operaciones de subcadena en texto amplio y columnas bytea, a cambio de mayor espacio de almacenamiento, ya que estas operaciones se optimizan para capturar solo los elementos requeridos del valor fuera de línea cuando no está comprimido.
- **Main** permite la compresión, pero no el almacenamiento fuera de línea. El almacenamiento fuera de línea aún se realizará para estas columnas, pero solo como último recurso cuando no haya ninguna otra manera de hacer que la fila sea lo suficientemente pequeña como para caber en una página.

## <a name="using-toast-table-storage-strategies"></a>Uso de estrategias de almacenamiento de tablas TOAST
Si sus consultas acceden a tipos de datos con capacidad TOAST, considere la posibilidad de usar **Main** en lugar de la opción predeterminada **Extended** para reducir los tiempos de consulta. **Main** no excluye el almacenamiento fuera de línea. Por otro lado, si las consultas no acceden a los tipos de datos con capacidad TOAST, puede ser ventajoso mantener la opción **Extended**. Por lo que una parte más grande de las filas de la tabla principal cabrán en la caché del búfer compartido, lo que ayuda al rendimiento.

Si tiene una carga de trabajo con un esquema con tablas anchas y gran recuento de caracteres, considere el uso de las tablas TOAST de PostgreSQL. Una tabla de ejemplo de un cliente tenía más de 350 columnas con varias columnas que abarcan 255 caracteres. Su tiempo de consulta de prueba comparativa se redujo de 4203 segundos a 467 segundos, una mejora del 89 por ciento, después de convertir la estrategia TOAST de **Main**.

## <a name="next-steps"></a>Pasos siguientes
Revise la carga de trabajo en busca de las características anteriores. 

Revise la siguiente documentación de PostgreSQL: [Capítulo 68, Almacenamiento físico de base de datos](https://www.postgresql.org/docs/current/storage-toast.html) 