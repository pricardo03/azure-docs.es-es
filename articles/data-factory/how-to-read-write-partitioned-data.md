---
title: Cómo leer o escribir datos con particiones en Azure Data Factory | Microsoft Docs
description: Aprenda a leer o escribir datos con particiones en Azure Data Factory versión 2.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: shlo
ms.openlocfilehash: 24464d110b00508cfb3fde4ab1a050773511e255
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "49091056"
---
# <a name="how-to-read-or-write-partitioned-data-in-azure-data-factory"></a>Lectura o escritura de datos con particiones en Azure Data Factory versión 2

En la versión 1 de Azure Data Factory, podrá leer y escribir datos con particiones por medio de las variables del sistema **SliceStart**, **SliceEnd**, **WindowStart** y **WindowEnd**. En la versión actual de Data Factory, este comportamiento se logra mediante un parámetro de canalización y la hora de inicio o la hora programada del desencadenador como valor del parámetro. 

## <a name="use-a-pipeline-parameter"></a>Usar un parámetro de canalización 

En la versión 1 de Data Factory, podía usar la propiedad **partitionedBy** y la variable del sistema **SliceStart** tal como se muestra en el ejemplo siguiente: 

```json
"folderPath": "adfcustomerprofilingsample/logs/marketingcampaigneffectiveness/{Year}/{Month}/{Day}/",
"partitionedBy": [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "%M" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "%d" } }
],
```

Para obtener más información acerca de la propiedad **partitonedBy**, vea [Copia de datos con Azure Blob Storage como origen o destino mediante Azure Data Factory](v1/data-factory-azure-blob-connector.md#dataset-properties). 

Para lograr este comportamiento en la versión actual de Data Factory: 

1. Defina un *parámetro de canalización* de tipo **cadena**. En el ejemplo siguiente, el nombre del parámetro de canalización es **windowStartTime**. 
2. Establezca la propiedad **folderPath** de la definición del conjunto de datos para que haga referencia al valor del parámetro de canalización. 
3. Pase el valor real para el parámetro al invocar la canalización a petición. También puede pasar una hora de inicio o programada del desencadenador de forma dinámica en tiempo de ejecución. 

```json
"folderPath": {
      "value": "adfcustomerprofilingsample/logs/marketingcampaigneffectiveness/@{formatDateTime(pipeline().parameters.windowStartTime, 'yyyy/MM/dd')}/",
      "type": "Expression"
},
```

## <a name="pass-in-a-value-from-a-trigger"></a>Pasar un valor desde un desencadenador

En la siguiente definición del desencadenador de la ventana de saltos de tamaño constante, la hora de inicio del desencadenador se pasa como un valor del parámetro de canalización **windowStartTime**: 

```json
{
    "name": "MyTrigger",
    "properties": {
        "type": "TumblingWindowTrigger",
        "typeProperties": {
            "frequency": "Hour",
            "interval": "1",
            "startTime": "2018-05-15T00:00:00Z",
            "delay": "00:10:00",
            "maxConcurrency": 10
        },
        "pipeline": {
            "pipelineReference": {
                "type": "PipelineReference",
                "referenceName": "MyPipeline"
            },
            "parameters": {
                "windowStartTime": "@trigger().outputs.windowStartTime"
            }
        }
    }
}
```

## <a name="example"></a>Ejemplo

Aquí puede ver una definición de datos de ejemplo:

```json
{
  "name": "SampleBlobDataset",
  "type": "AzureBlob",
  "typeProperties": {
    "folderPath": {
      "value": "adfcustomerprofilingsample/logs/marketingcampaigneffectiveness/@{formatDateTime(pipeline().parameters.windowStartTime, 'yyyy/MM/dd')}/",
      "type": "Expression"
    },
    "format": {
      "type": "TextFormat",
      "columnDelimiter": ","
    }
  },
  "structure": [
    { "name": "ProfileID", "type": "String" },
    { "name": "SessionStart", "type": "String" },
    { "name": "Duration", "type": "Int32" },
    { "name": "State", "type": "String" },
    { "name": "SrcIPAddress", "type": "String" },
    { "name": "GameType", "type": "String" },
    { "name": "Multiplayer", "type": "String" },
    { "name": "EndRank", "type": "String" },
    { "name": "WeaponsUsed", "type": "Int32" },
    { "name": "UsersInteractedWith", "type": "String" },
    { "name": "Impressions", "type": "String" }
  ],
  "linkedServiceName": {
    "referenceName": "churnStorageLinkedService",
    "type": "LinkedServiceReference"
  }
}
```

Definición de la canalización: 

```json
{
    "properties": {
        "activities": [{
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": {
                    "value": "@concat(pipeline().parameters.blobContainer, '/scripts/', pipeline().parameters.partitionHiveScriptFile)",
                    "type": "Expression"
                },
                "scriptLinkedService": {
                    "referenceName": "churnStorageLinkedService",
                    "type": "LinkedServiceReference"
                },
                "defines": {
                    "RAWINPUT": {
                        "value": "@concat('wasb://', pipeline().parameters.blobContainer, '@', pipeline().parameters.blobStorageAccount, '.blob.core.windows.net/logs/', pipeline().parameters.inputRawLogsFolder, '/')",
                        "type": "Expression"
                    },
                    "Year": {
                        "value": "@formatDateTime(pipeline().parameters.windowStartTime, 'yyyy')",
                        "type": "Expression"
                    },
                    "Month": {
                        "value": "@formatDateTime(pipeline().parameters.windowStartTime, 'MM')",
                        "type": "Expression"
                    },
                    "Day": {
                        "value": "@formatDateTime(pipeline().parameters.windowStartTime, 'dd')",
                        "type": "Expression"
                    }
                }
            },
            "linkedServiceName": {
                "referenceName": "HdiLinkedService",
                "type": "LinkedServiceReference"
            },
            "name": "HivePartitionGameLogs"
        }],
        "parameters": {
            "windowStartTime": {
                "type": "String"
            },
            "blobStorageAccount": {
                "type": "String"
            },
            "blobContainer": {
                "type": "String"
            },
            "inputRawLogsFolder": {
                "type": "String"
            }
        }
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener un tutorial completo acerca de cómo crear una factoría de datos con una canalización, consulte [Inicio rápido: Crear una factoría de datos](quickstart-create-data-factory-powershell.md). 

