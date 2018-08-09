---
title: Referencia rápida de los comandos de trabajos de importación de la herramienta Azure Import/Export | Microsoft Docs
description: Referencia sobre los comandos de la herramienta de Azure Import/Export para comandos de trabajos de importación usados con frecuencia.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: 8f3eb2bf2d9789b678849f9e2415816d15afc29e
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39526637"
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a>Referencia rápida de comandos usados con frecuencia para trabajos de importación

En este artículo se proporciona una referencia rápida para algunos comandos que se usan con frecuencia. Para obtener información detallada sobre el uso, vea [Preparing Hard Drives for an Import Job](../storage-import-export-tool-preparing-hard-drives-import.md) (Preparación de los discos duros para un trabajo de importación).

## <a name="first-session"></a>Primera sesión

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#1 /sk:************* /InitialDriveSet:driveset-1.csv /DataSet:dataset-1.csv /logdir:F:\logs
```

## <a name="second-session"></a>Segunda sesión

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /DataSet:dataset-2.csv
```

## <a name="abort-latest-session"></a>Anulación de la última sesión

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#2 /AbortSession
```

## <a name="resume-latest-interrupted-session"></a>Reanudación de la última sesión interrumpida

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3 /ResumeSession
```

## <a name="add-drives-to-latest-session"></a>Adición de unidades a la última sesión

```
WAImportExport.exe PrepImport /j:JournalTest.jrn /id:session#3 /AdditionalDriveSet:driveset-2.csv
```

## <a name="next-steps"></a>Pasos siguientes

* [Flujo de trabajo de ejemplo para preparar las unidades de disco duro para un trabajo de importación](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow.md)
