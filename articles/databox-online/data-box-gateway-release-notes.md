---
title: Notas de la versión preliminar de Azure Data Box Gateway | Microsoft Docs
description: Describe los problemas críticos y las soluciones para la versión preliminar de Azure Data Box Gateway.
services: databox-edge-gateway
documentationcenter: ''
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: 7619056ace5d9b3cf083a40a6cfa06a0cac0561e
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46949696"
---
# <a name="azure-data-box-gateway-preview-release-notes"></a>Notas de la versión preliminar de Azure Data Box Gateway

## <a name="overview"></a>Información general

En las notas de la versión siguientes se identifican los problemas críticos pendientes y los problemas resueltos de las actualizaciones de la versión preliminar de Azure Data Box Gateway.

Las notas de la versión se actualizan continuamente y se van agregando a medida que se descubren problemas críticos que requieren una solución alternativa. Antes de implementar Data Box Gateway, le recomendamos que lea detenidamente la información que encontrará en las notas de la versión.

La versión preliminar se corresponde con la versión de software **Data Box Gateway (versión preliminar) versión 2.0**.

## <a name="issues-fixed-in-preview-release"></a>Problemas corregidos en la versión preliminar

En la tabla siguiente se proporciona un resumen de los problemas corregidos en esta versión.

| No. | Problema |
| --- | --- |
| 1 | En esta versión, cuando se actualiza un archivo que se ha cargado con otra herramienta (AzCopy) y después se actualiza de forma que aumenta el tamaño del archivo, se produce el siguiente error: *Error 400: InvalidBlobOrBlock (el contenido en blobs o bloques especificado no es válido)*.|


## <a name="known-issues-in-preview-release"></a>Problemas conocidos en la versión preliminar

En la tabla siguiente se proporciona un resumen de los problemas conocidos de la versión preliminar de Data Box Gateway.

| No. | Característica | Problema | Soluciones alternativas o comentarios |
| --- | --- | --- | --- |
| **1.** |Actualizaciones |Los dispositivos de Data Box Gateway creados en las versiones preliminares anteriores se pueden actualizar a esta versión. |Descargar las imágenes de disco virtual de la nueva versión y configure e implemente nuevos dispositivos. Para más información, consulte [Preparación de la implementación de Azure Data Box Gateway](data-box-gateway-deploy-prep.md). |
| **2.** |Disco de datos aprovisionado |Después de aprovisionar un disco de datos de un determinado tamaño y de crear la correspondiente instancia de Data Box Gateway, no debe reducir el disco de datos. Si intenta hacer esto, se perderán todos los datos locales del dispositivo. | |
| **3.** |Actualizar |En esta versión, puede actualizar solo un recurso compartido a la vez. | |
| **4.** |Cambiar nombre |No se permite cambiar el nombre de los objetos. |Si esta característica es fundamental para su flujo de trabajo, póngase en contacto con el Soporte técnico de Microsoft. |
| **5.** |Copiar| Si se copia un archivo de solo lectura en el dispositivo, no se conserva la propiedad de solo lectura. | |
| **6.** |Registros| Debido a un error en esta versión, podría ver instancias del código de error 110 en *error.xml* con nombres de elemento no reconocibles. | |
| **7.** |Cargar | Debido a un error en esta versión, podría ver instancias del código de error 2003 durante la carga de archivos específicos. | |
| **8.** |Eliminación | Debido a un error en esta versión, si se elimina un recurso compartido NFS, es posible que el recurso compartido no se pueda eliminar. El estado del recurso compartido será *Deleting* (Eliminando).  |Esto solo se produce cuando el recurso compartido usa un nombre de archivo no admitido. |
| **9.** |Ayuda en línea |Los vínculos de Ayuda en Azure Portal podrían no conducir a la documentación.|Los vínculos de Ayuda funcionarán en la versión de disponibilidad general. |



## <a name="next-steps"></a>Pasos siguientes

- [Preparación de la implementación de Azure Data Box Gateway](data-box-gateway-deploy-prep.md)


