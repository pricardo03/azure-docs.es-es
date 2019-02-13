---
title: Conector de Azure Data Explorer para Apache Spark
description: En este artículo se muestra cómo usar el conector de Azure Data Explorer para Apache Spark.
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 02/05/2018
ms.openlocfilehash: 6f115df97d0b790c94d6dc07f3f40feeceb1a7cc
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55825192"
---
# <a name="azure-data-explorer-connector-for-apache-spark"></a>Conector de Azure Data Explorer para Apache Spark

Con Azure Data Explorer y Apache Spark, se pueden crear aplicaciones rápidas y escalables orientadas a escenarios controlados por datos, tales como el aprendizaje automático, la extracción, transformación y carga de datos (ETL), y Log Analytics.

## <a name="prerequisites"></a>Requisitos previos

Para usar el conector, la aplicación debe tener:

* Java 1.8 SDK
* Maven 3.x
* Versión 2.3.2 o superior de Spark

## <a name="link-to-data-explorer"></a>Vínculo a Data Explorer

Para aplicaciones Scala y Java que utilizan definiciones de proyecto Maven, vincule la aplicación con el siguiente artefacto:

```java
groupId = com.microsoft.azure
artifactId = spark-kusto-connector
version = 1.0.0-Beta-01 
```

En Maven, realice la vinculación con lo siguiente:

```Maven
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spark-kusto-connector</artifactId>
    <version>1.0.0-Beta-01</version>
  </dependency>
```

## <a name="build-commands"></a>Comandos de compilación

Para compilar el archivo jar y ejecutar todas las pruebas:

```
mvn clean package
```

Para compilar el archivo jar, ejecutar todas las pruebas e instalar jar en el repositorio de Maven local:

```
mvn clean install
```