---
title: 'Datos de ejemplo en diferentes ubicaciones de almacenamiento de Azure: proceso de ciencia de datos en equipos'
description: Datos de ejemplo en contenedores de blob de Azure, SQL Server y tablas de Hive para reducirlos a un tamaño más pequeño pero representativo y más manejable.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: fc91aec80f74488125649cfe807757ba5ae49c9b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60253093"
---
# <a name="heading"></a>Muestra de datos en contenedores de blob de Azure, SQL Server y tablas de Hive

Los artículos siguientes describen cómo realizar un muestreo de los datos que se almacenan en una de las tres diferentes ubicaciones de Azure:

* Para muestrear los [**datos del contenedor de blobs de Azure**](sample-data-blob.md), se descargan mediante programación y, a continuación, se realiza un muestreo de ellos con el código Python de ejemplo.
* Los [**datos de SQL Server**](sample-data-sql-server.md) se muestrean tanto con SQL como con el lenguaje de programación de Python. 
* Los [**datos de las tablas de Hive**](sample-data-hive.md) se muestrean con consultas de Hive.

Esta tarea de muestreo es un paso en el [proceso de ciencia de datos en equipos (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/).

**Razones para el uso de datos de ejemplo**

Si el conjunto de datos que pretende analizar es grande, es recomendable reducirlo a un tamaño más pequeño, pero representativo, que sea más manejable. Esto facilita la comprensión y exploración de los datos, y el diseño de características. Su rol en el proceso de análisis de Cortana es permitir la rápida creación de prototipos de las funciones de procesamiento de datos y de los modelos de aprendizaje automático.

