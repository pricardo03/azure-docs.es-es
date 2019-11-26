---
title: 'Carga de datos en entornos de Azure Storage: proceso de ciencia de datos en equipos'
description: Obtenga información sobre cómo ingerir datos en varios entornos de destino donde los datos se almacenan y se procesan.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/09/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 515decfafe46ad0c1b5b90743688abc26a975903
ms.sourcegitcommit: 87efc325493b1cae546e4cc4b89d9a5e3df94d31
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2019
ms.locfileid: "73053256"
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
