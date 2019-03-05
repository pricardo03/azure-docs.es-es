---
title: Copia incremental de datos con Azure Data Factory | Microsoft Docs
description: En estos tutoriales se muestra cómo copiar datos de forma incremental de un almacén de datos de origen a un almacén de datos de destino. La primera de ellas copia los datos de una tabla.
services: data-factory
documentationcenter: ''
author: dearandyxu
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/22/2018
ms.author: yexu
ms.openlocfilehash: cb75e943416c227730589ab5e7feeb7b8ba5e245
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "56957936"
---
# <a name="incrementally-load-data-from-a-source-data-store-to-a-destination-data-store"></a>Carga incremental de datos de un almacén de datos de origen a un almacén de datos de destino

En una solución de integración de datos, la carga incremental (o diferencial) de los datos después de una carga completa de los datos es un método ampliamente usado. En los tutoriales de esta sección se muestran distintas formas de cargar datos de forma incremental mediante Azure Data Factory.

## <a name="delta-data-loading-from-database-by-using-a-watermark"></a>Carga diferencial de datos de la base de datos mediante una marca de agua
En este caso, definirá una marca de agua en la base de datos de origen. Una marca de agua es una columna que tiene la marca de tiempo de la última actualización o una clave de incremento. La solución de carga diferencial carga los datos modificados entre una marca de agua antigua y una nueva marca de agua. En el siguiente diagrama se representa el flujo de trabajo de este enfoque: 

![Flujo de trabajo de uso de una marca de agua](media/tutorial-incremental-copy-overview/workflow-using-watermark.png)

Consulte los siguientes temas para obtener instrucciones paso a paso: 

- [Carga de datos de forma incremental de Azure SQL Database a Azure Blob Storage](tutorial-incremental-copy-powershell.md)
- [Carga incremental de datos de varias tablas de SQL Server a Azure SQL Database](tutorial-incremental-copy-multiple-tables-powershell.md)

## <a name="delta-data-loading-from-sql-db-by-using-the-change-tracking-technology"></a>Carga diferencial de datos SQL DB mediante la tecnología Change Tracking
La tecnología de control de cambios es una solución ligera de SQL Server y Azure SQL Database que ofrece un mecanismo eficaz de control de cambios para las aplicaciones. Así, permite que una aplicación identifique fácilmente los datos que se insertaron, actualizaron o eliminaron. 

En el siguiente diagrama se representa el flujo de trabajo de este enfoque:

![Flujo de trabajo del control de cambios](media/tutorial-incremental-copy-overview/workflow-using-change-tracking.png)

Para ver instrucciones paso a paso, consulte el siguiente tutorial: <br/>
[Carga incremental de datos de Azure SQL Database a Azure Blob Storage mediante la información de control de cambios](tutorial-incremental-copy-change-tracking-feature-powershell.md)

## <a name="loading-new-and-changed-files-only-by-using-lastmodifieddate"></a>Carga de archivos nuevos y modificados solo mediante LastModifiedDate
Puede obtener primero los metadatos (LastModifiedDate) de los archivos y, luego, copiar los archivos nuevos y modificados solo en el almacén de destino.

Para ver instrucciones paso a paso, consulte el siguiente tutorial: <br/>
[Copia incremental de archivos nuevos y modificados según LastModifiedDate desde Azure Blob Storage hasta Azure Blob Storage](tutorial-incremental-copy-lastmodified-copy-data-tool.md)

## <a name="loading-new-files-only-by-using-time-partitioned-folder-or-file-name"></a>Carga de archivos nuevos mediante únicamente el nombre de archivo o la carpeta con particiones de tiempo
Puede copiar solamente archivos nuevos, donde ya se ha realizado una partición de tiempo de los archivos o carpetas con información de intervalo de tiempo como parte del nombre de archivo o carpeta (por ejemplo, /aaaa/mm/dd/file.csv). 

Para ver instrucciones paso a paso, consulte el siguiente tutorial: <br/>
[Copia incremental de nuevos archivos según la carpeta con particiones de tiempo o el nombre de archivo desde Azure Blob Storage hasta Azure Blob Storage](tutorial-incremental-copy-partitioned-file-name-copy-data-tool.md)

## <a name="next-steps"></a>Pasos siguientes
Avance al siguiente tutorial: 

> [!div class="nextstepaction"]
>[Carga de datos de forma incremental de Azure SQL Database a Azure Blob Storage](tutorial-incremental-copy-powershell.md)
