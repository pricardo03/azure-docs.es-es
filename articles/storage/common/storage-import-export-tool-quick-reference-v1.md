---
title: Referencia rápida de los comandos de trabajos de importación de la herramienta Azure Import/Export (versión 1) | Microsoft Docs
description: Referencia sobre los comandos de la herramienta de Azure Import/Export para comandos de trabajos de importación usados con frecuencia. Se refiere a la versión 1 de la herramienta Import/Export.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/15/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: 16e0fa6f7336b39f63b00564d37c1be308bebb16
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39526290"
---
# <a name="quick-reference-for-frequently-used-commands-for-import-jobs"></a>Referencia rápida de comandos usados con frecuencia para trabajos de importación
En esta sección se proporciona una referencia rápida para algunos comandos que se usan con frecuencia. Para obtener información detallada sobre el uso, vea [Preparing Hard Drives for an Import Job](../storage-import-export-tool-preparing-hard-drives-import-v1.md) (Preparación de los discos duros para un trabajo de importación).  

## <a name="prepare-a-hard-drive-when-data-has-already-been-copied-to-the-hard-drive"></a>Preparación de un disco duro cuando los datos ya se han copiado en el disco duro
 El siguiente comando prepara una unidad de disco duro cuando los datos ya se han copiado en ella, pero aún no se han cifrado con BitLocker:  
  
```  
  WAImportExport.exe PrepImport /j:9WM35C2V.jrn /id:session#1 /sk:VkGbrUqBWLYJ6zg1m29VOTrxpBgdNOlp+kp0C9MEdx3GELxmBw4hK94f7KysbbeKLDksg7VoN1W/a5UuM2zNgQ== /t:d /encrypt /srcdir:d:\movies\drama /dstdir:movies/drama/ /skipwrite
```    

## <a name="copy-a-single-directory-to-a-hard-drive"></a>Copia de un único directorio en una unidad de disco duro  
 El comando siguiente copia un único directorio de origen en una unidad de disco duro que aún no se ha cifrado con BitLocker:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
## <a name="copy-two-directories-to-a-hard-drive"></a>Copia de dos directorios en una unidad de disco duro  
 Para copiar dos directorios de origen en una unidad, use los comandos siguientes:  
  
 El primero especifica el directorio de registro, la clave de cuenta de almacenamiento, la letra de la unidad de destino y los requisitos de `format/encrypt`, además de los parámetros comunes:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:movies /logdir:c:\logs /sk:8ImTigJhIwvL9VEIQKB/zbqcXbxrIHbBjLIfOt0tyR98TxtFvUM/7T0KVNR6KRkJrh26u5I8hTxTLM2O1aDVqg== /t:x /format /encrypt /srcdir:d:\Movies /dstdir:entertainment/movies/  
```  
  
 El segundo comando especifica el archivo de diario, un nuevo identificador de sesión y las ubicaciones de origen y de destino:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:music /srcdir:d:\Music /dstdir:entertainment/music/  
```  
  
## <a name="copy-a-large-file-to-a-hard-drive-in-a-second-copy-session"></a>Copia de un archivo grande en una unidad de disco duro en una segunda sesión de copia  
 El comando siguiente copia un único archivo grande en una unidad de disco duro que se ha preparado en una sesión de copia anterior:  
  
```  
WAImportExport.exe PrepImport /j:FirstDrive.jrn /id:dvd /srcfile:d:\dvd\favoritemovie.vhd /dstblob:dvd/favoritemovie.vhd  
```  
  
## <a name="next-steps"></a>Pasos siguientes

* [Flujo de trabajo de ejemplo para preparar las unidades de disco duro para un trabajo de importación](storage-import-export-tool-sample-preparing-hard-drives-import-job-workflow-v1.md)
