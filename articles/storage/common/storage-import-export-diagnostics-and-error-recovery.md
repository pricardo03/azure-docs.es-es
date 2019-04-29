---
title: Diagnóstico y recuperación de errores para los trabajos de Azure Import/Export | Microsoft Docs
description: Obtenga información sobre cómo habilitar el registro detallado para los trabajos del servicio Microsoft Azure Import/Export.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 306b94fbe23e0ae92dcd59f7a87b7bb58ef7c3b6
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61478817"
---
# <a name="diagnostics-and-error-recovery-for-azure-importexport-jobs"></a>Diagnóstico y recuperación de errores de los trabajos de Azure Import/Export
Para cada unidad de disco procesada, el servicio Azure Import/Export crea un registro de errores en la cuenta de almacenamiento asociada. También puede habilitar el registro detallado estableciendo la propiedad `LogLevel` en `Verbose` al llamar a las operaciones [Put Job](/rest/api/storageimportexport/jobs) o [Update Job Properties](/rest/api/storageimportexport/jobs).

 De forma predeterminada, los registros se escriben en un contenedor denominado `waimportexport`. Puede especificar un nombre distinto estableciendo la propiedad `DiagnosticsPath` al llamar a las operaciones `Put Job` o `Update Job Properties`. Los registros se almacenan como blobs en bloques con la convención de nomenclatura siguiente: `waies/jobname_driveid_timestamp_logtype.xml`.

 Puede recuperar el identificador URI de los registros de un trabajo llamando a la operación [Get Job](/rest/api/storageimportexport/jobs). El identificador URI para el registro detallado se devuelve en la propiedad `VerboseLogUri` para cada unidad, mientras que el identificador URI para el registro de errores se devuelve en la propiedad `ErrorLogUri`.

Puede usar los datos de registro para identificar los problemas siguientes.

## <a name="drive-errors"></a>Errores en la unidad

Los siguientes elementos se clasifican como errores en la unidad:

-   Errores de acceso o lectura en el archivo de manifiesto

-   Claves de BitLocker incorrectas

-   Errores de escritura/lectura en la unidad

## <a name="blob-errors"></a>Errores de blobs

Los siguientes elementos se clasifican como errores de blob:

-   Nombres o blobs incorrectos o conflictivos

-   Archivos que faltan

-   Blob no encontrado

-   Archivos truncados (los archivos del disco son más pequeños de lo especificado en el manifiesto)

-   Contenido de archivo dañado (para los trabajos de importación, se ha detectado una incoherencia de suma de comprobación MD5)

-   Archivos de metadatos y propiedades de blobs dañados (detectados con una incoherencia de suma de comprobación MD5)

-   Esquema incorrecto para las propiedades de blob o los archivos de metadatos

Puede haber casos donde algunas partes de un trabajo de importación o exportación no finalizan correctamente, mientras que el trabajo completo sí finaliza. En este caso, puede cargar o descargar los elementos que faltan de los datos a través de la red, o puede crear un nuevo trabajo para transferir los datos. Vea [Referencia de la herramienta Azure Import/Export](storage-import-export-tool-how-to-v1.md) para obtener información sobre cómo reparar los datos a través de la red.

## <a name="next-steps"></a>Pasos siguientes

* [Uso de la API de REST del servicio Azure Import/Export](storage-import-export-using-the-rest-api.md)
