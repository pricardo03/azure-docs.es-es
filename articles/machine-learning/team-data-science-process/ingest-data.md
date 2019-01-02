---
title: 'Carga de datos en entornos de Azure Storage: proceso de ciencia de datos en equipos'
description: Mover datos hacia y desde Azure Blob Storage
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/09/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: b53ece6cd2e61fa6f6340b7bda3021ee7d9d767c
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53139714"
---
# <a name="load-data-into-storage-environments-for-analytics"></a>Carga de datos en entornos de almacenamiento para el análisis

El proceso de ciencia de datos en equipos requiere que los datos se introduzcan o se cargue en una variedad de entornos de almacenamiento diferentes para que se procesen o analicen de la manera más adecuada en cada fase del proceso. Entre los destinos de datos más usados para el procesamiento se incluyen el almacenamiento de blobs de Azure, las bases de datos SQL Azure, SQL Server en VM de Azure, HDInsight (Hadoop) y Azure Machine Learning. 

En los artículos siguientes se describe cómo introducir datos en varios entornos de destino donde los datos se almacenan y se procesan.

* Hacia o desde [Azure Blob Storage](move-azure-blob.md)
* Hacia [SQL Server en máquinas virtuales de Azure](move-sql-server-virtual-machine.md)
* Hacia [Azure SQL Database](move-sql-azure.md)
* Hacia [tablas de Hive](move-hive-tables.md)
* Hacia [tablas SQL con particiones](parallel-load-sql-partitioned-tables.md)
* Desde [SQL Server local](move-sql-azure-adf.md)

Las necesidades técnicas y empresariales, así como la ubicación inicial, el formato y el tamaño de los datos determinarán los entornos de destino en los que deben introducirse los datos para lograr los objetivos de su análisis. No es raro que un escenario requiera que los datos se muevan entre varios entornos para lograr la variedad de las tareas necesarias para construir un modelo predictivo. En esta secuencia de tareas se puede incluir, por ejemplo, la exploración de datos, el procesamiento previo, la limpieza, el muestreo inferior y el entrenamiento del modelo.
