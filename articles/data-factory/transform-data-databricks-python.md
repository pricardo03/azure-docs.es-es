---
title: 'Transformación de datos con Python en Databricks: Azure'
description: Obtenga información sobre cómo procesar o transformar datos mediante la ejecución de una instancia de Python en Databricks.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 03/15/2018
author: djpmsft
ms.author: daperlov
ms.reviewer: maghan
manager: craigg
ms.openlocfilehash: 47654e8183dab120376f94df63d7664ead5dd580
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73683935"
---
# <a name="transform-data-by-running-a-python-activity-in-azure-databricks"></a>Transformación de datos mediante la ejecución de una actividad de Python en Azure Databricks

La actividad de Python en Azure Databricks en una [canalización de Data Factory](concepts-pipelines-activities.md) ejecuta un archivo de Python en el clúster de Azure Databricks. Este artículo se basa en el artículo sobre  [actividades de transformación de datos](transform-data.md) , que presenta información general de la transformación de datos y las actividades de transformación admitidas. Azure Databricks es una plataforma administrada para ejecutar Apache Spark.

Si desea una introducción y demostración de once minutos de esta característica, vea el siguiente vídeo:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Execute-Jars-and-Python-scripts-on-Azure-Databricks-using-Data-Factory/player]

## <a name="databricks-python-activity-definition"></a>Definición de la actividad de Python en Databricks

Esta es la definición de JSON de ejemplo de una actividad de Python en Databricks:

```json
{
    "activity": {
        "name": "MyActivity",
        "description": "MyActivity description",
        "type": "DatabricksSparkPython",
        "linkedServiceName": {
            "referenceName": "MyDatabricksLinkedservice",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "pythonFile": "dbfs:/docs/pi.py",
            "parameters": [
                "10"
            ],
            "libraries": [
                {
                    "pypi": {
                        "package": "tensorflow"
                    }
                }
            ]
        }
    }
}
```

## <a name="databricks-python-activity-properties"></a>Propiedades de la actividad de Python en Databricks

En la siguiente tabla se describen las propiedades JSON que se usan en la definición de JSON:

|Propiedad|DESCRIPCIÓN|Obligatorio|
|---|---|---|
|name|Nombre de la actividad en la canalización.|Sí|
|description|Texto que describe para qué se usa la actividad.|Sin|
|Tipo|En el caso de la actividad de Python en Databricks, el tipo de actividad es DatabricksSparkPython.|Sí|
|linkedServiceName|Nombre del servicio vinculado de Databricks en el que se ejecuta la actividad de Python. Para más información sobre este servicio vinculado, consulte el artículo  [Compute linked services](compute-linked-services.md)  (Servicios vinculados de procesos).|Sí|
|pythonFile|El URI del archivo Python que se va a ejecutar. Solo se admiten rutas de acceso de DBFS.|Sí|
|parameters|Parámetros de la línea de comandos que se pasarán al archivo Python. Se trata de una matriz de cadenas.|Sin|
|libraries|Lista de bibliotecas para instalar en el clúster que ejecutará el trabajo. Puede ser una cadena de <cadena, objeto>|Sin|

## <a name="supported-libraries-for-databricks-activities"></a>Bibliotecas compatibles con las actividades de Databricks

En la definición de la actividad de Databricks anterior, especifica estos tipos de biblioteca: *jar*, *egg*, *maven*, *pypi*, *cran*.

```json
{
    "libraries": [
        {
            "jar": "dbfs:/mnt/libraries/library.jar"
        },
        {
            "egg": "dbfs:/mnt/libraries/library.egg"
        },
        {
            "maven": {
                "coordinates": "org.jsoup:jsoup:1.7.2",
                "exclusions": [ "slf4j:slf4j" ]
            }
        },
        {
            "pypi": {
                "package": "simplejson",
                "repo": "http://my-pypi-mirror.com"
            }
        },
        {
            "cran": {
                "package": "ada",
                "repo": "https://cran.us.r-project.org"
            }
        }
    ]
}

```

Para más detalles, consulte la [documentación de Databricks](https://docs.azuredatabricks.net/api/latest/libraries.html#managedlibrarieslibrary) sobre los tipos de bibliotecas.

## <a name="how-to-upload-a-library-in-databricks"></a>Carga de una biblioteca en Databricks

#### <a name="using-databricks-workspace-uihttpsdocsazuredatabricksnetuser-guidelibrarieshtmlcreate-a-library"></a>[Uso de la interfaz de usuario del área de trabajo de Databricks](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library)

Para obtener la ruta de acceso de dbfs de la biblioteca que se agregó mediante la interfaz de usuario, puede usar la [CLI de Databricks (instalación)](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli). 

Habitualmente, las bibliotecas de Jar se almacenan en dbfs:/FileStore/jars mientras se usa la interfaz de usuario. Puede enumerar todo a través de la CLI: *databricks fs ls dbfs:/FileStore/jars* 



#### <a name="copy-library-using-databricks-clihttpsdocsazuredatabricksnetuser-guidedev-toolsdatabricks-clihtmlcopy-a-file-to-dbfs"></a>[Copia de la biblioteca mediante la CLI de Databricks](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#copy-a-file-to-dbfs)

Ejemplo: *databricks fs cp SparkPi-assembly-0.1.jar dbfs:/FileStore/jars*