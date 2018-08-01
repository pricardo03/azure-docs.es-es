---
title: 'Transformación de datos con cuadernos de Databricks: Azure | Microsoft Docs'
description: Aprenda a procesar o transformar datos mediante la ejecución de blocs de notas de Databricks.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.assetid: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/15/2018
ms.author: douglasl
ms.openlocfilehash: 5f21f33678b8cf09d9dbd8966d42b1a5ebac9ffb
ms.sourcegitcommit: 44fa77f66fb68e084d7175a3f07d269dcc04016f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2018
ms.locfileid: "39224659"
---
# <a name="transform-data-by-running-a-databricks-notebook"></a>Transformación de datos mediante la ejecución de blocs de notas de Databricks

La actividad Notebook de Azure Databricks en una [canalización de Data Factory](concepts-pipelines-activities.md) ejecuta un bloc de notas de Databricks en el área de trabajo de Azure Databricks. Este artículo se basa en el artículo sobre [actividades de transformación de datos](transform-data.md) , que presenta información general de la transformación de datos y las actividades de transformación admitidas. Azure Databricks es una plataforma administrada para ejecutar Apache Spark.

## <a name="databricks-notebook-activity-definition"></a>Definición de la actividad Notebook de Databricks

Esta es la definición de JSON de ejemplo de una actividad Notebook de Databricks:

```json
{
    "activity": {
        "name": "MyActivity",
        "description": "MyActivity description",
        "type": "DatabricksNotebook",
        "linkedServiceName": {
            "referenceName": "MyDatabricksLinkedservice",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "notebookPath": "/Users/user@example.com/ScalaExampleNotebook",
            "baseParameters": {
                "inputpath": "input/folder1/",
                "outputpath": "output/"
            },
            "libraries": [
                {
                "jar": "dbfs:/docs/library.jar"
                }
            ]
        }
    }
}
```

## <a name="databricks-notebook-activity-properties"></a>Propiedades de la actividad Notebook de Databricks

En la siguiente tabla se describen las propiedades JSON que se usan en la definición de JSON:

|Propiedad|DESCRIPCIÓN|Obligatorio|
|---|---|---|
|Nombre|Nombre de la actividad en la canalización.|SÍ|
|Descripción|Texto que describe para qué se usa la actividad.|Sin |
|Tipo|Para la actividad Notebook de Databricks, el tipo de actividad es DatabricksNotebook.|SÍ|
|linkedServiceName|Nombre del servicio vinculado de Databricks en el que se ejecuta el bloc de notas de Databricks. Para obtener más información sobre este servicio vinculado, vea el artículo [Compute linked services](compute-linked-services.md) (Servicios vinculados de procesos).|SÍ|
|notebookPath|La ruta de acceso absoluta del cuaderno que se va a ejecutar en el área de trabajo de Databricks. Esta ruta de acceso debe comenzar con una barra diagonal.|SÍ|
|baseParameters|Una matriz de pares de clave y valor. Se pueden utilizar parámetros base para cada ejecución de actividad. Si el cuaderno toma un parámetro que no se ha especificado, se usará el valor predeterminado del cuaderno. Encuentre más información sobre los parámetros en los [cuadernos de Databricks](https://docs.databricks.com/api/latest/jobs.html#jobsparampair).|Sin |
|libraries|Lista de bibliotecas para instalar en el clúster que ejecutará el trabajo. Puede ser una matriz de \<cadena, objeto>.|Sin |


## <a name="supported-libraries-for-databricks-activities"></a>Bibliotecas compatibles con las actividades de Databricks

En la definición de la actividad de Databricks anterior, se especifican estos tipos de biblioteca: *jar*, *egg*, *maven*, *pypi* y *cran*.

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
                "repo": "http://cran.us.r-project.org"
            }
        }
    ]
}

```

Para más detalles, consulte la [documentación de Databricks](https://docs.azuredatabricks.net/api/latest/libraries.html#managedlibrarieslibrary) sobre los tipos de bibliotecas.

## <a name="how-to-upload-a-library-in-databricks"></a>Carga de una biblioteca en Databricks

#### <a name="using-databricks-workspace-uihttpsdocsazuredatabricksnetuser-guidelibrarieshtmlcreate-a-library"></a>[Uso de la interfaz de usuario del área de trabajo de Databricks](https://docs.azuredatabricks.net/user-guide/libraries.html#create-a-library)

Para obtener la ruta de acceso de dbfs de la biblioteca que se agregó mediante la interfaz de usuario, puede usar la [CLI de Databricks (instalación)](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#install-the-cli). 

Habitualmente, las bibliotecas de Jar se almacenan en dbfs:/FileStore/jars mientras se usa la interfaz de usuario. Puede enumerar todo a través de la CLI: *databricks fs ls dbfs:/FileStore/jars*.



#### <a name="copy-library-using-databricks-clihttpsdocsazuredatabricksnetuser-guidedev-toolsdatabricks-clihtmlcopy-a-file-to-dbfs"></a>[Copia de la biblioteca mediante la CLI de Databricks](https://docs.azuredatabricks.net/user-guide/dev-tools/databricks-cli.html#copy-a-file-to-dbfs)

Ejemplo: *databricks fs cp SparkPi-assembly-0.1.jar dbfs:/FileStore/jars*
