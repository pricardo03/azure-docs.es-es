---
title: Muestra de datos en contenedores de blob de Azure, SQL Server y tablas de Hive | Microsoft Docs
description: Exploración de datos almacenados en diversos entornos de Azure
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.component: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: (previous author=deguhath, ms.author=deguhath)
ms.openlocfilehash: b7b233532a526879e886950c3a0d8cfe169251a7
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "52441509"
---
# <a name="heading"></a>Muestra de datos en contenedores de blob de Azure, SQL Server y tablas de Hive

Los artículos siguientes describen cómo realizar un muestreo de los datos que se almacenan en una de las tres diferentes ubicaciones de Azure:

* Para muestrear los [**datos del contenedor de blobs de Azure**](sample-data-blob.md), se descargan mediante programación y, a continuación, se realiza un muestreo de ellos con el código Python de ejemplo.
* Los [**datos de SQL Server**](sample-data-sql-server.md) se muestrean tanto con SQL como con el lenguaje de programación de Python. 
* Los [**datos de las tablas de Hive**](sample-data-hive.md) se muestrean con consultas de Hive.

Esta tarea de muestreo es un paso en el [proceso de ciencia de datos en equipos (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

**Razones para el uso de datos de ejemplo**

Si el conjunto de datos que pretende analizar es grande, es recomendable reducirlo a un tamaño más pequeño, pero representativo, que sea más manejable. Esto facilita la comprensión y exploración de los datos, y el diseño de características. Su rol en el proceso de análisis de Cortana es permitir la rápida creación de prototipos de las funciones de procesamiento de datos y de los modelos de aprendizaje automático.

