---
title: Notas de la versión preliminar de Azure Data Box Gateway | Microsoft Docs
description: Describe los problemas críticos y las soluciones para la versión preliminar de Azure Data Box Gateway.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 02/07/2019
ms.author: alkohli
ms.openlocfilehash: 0265de5b224e62d188fe6e3b9322d5c2e3f77fa1
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55883141"
---
# <a name="azure-data-box-gateway-preview-release-notes"></a>Notas de la versión preliminar de Azure Data Box Gateway

## <a name="overview"></a>Información general

En las notas de la versión siguientes se identifican los problemas críticos pendientes y los problemas resueltos de las actualizaciones de la versión preliminar de Azure Data Box Gateway.

Las notas de la versión se actualizan continuamente y se van agregando a medida que se descubren problemas críticos que requieren una solución alternativa. Antes de implementar Data Box Gateway, le recomendamos que lea detenidamente la información que encontrará en las notas de la versión.

La versión preliminar se corresponde con la versión de software **Data Box Gateway (versión preliminar) versión 2.0**.

## <a name="issues-fixed-in-preview-release"></a>Problemas corregidos en la versión preliminar

En la tabla siguiente se proporciona un resumen de los problemas corregidos en esta versión.

|  No. | Problema |
| --- | --- |
| **1.** | En esta versión, cuando se actualiza un archivo que se ha cargado con otra herramienta (AzCopy) y después se actualiza de forma que aumenta el tamaño del archivo, se produce el siguiente error: *Error 400: InvalidBlobOrBlock (el contenido en blobs o bloques especificado no es válido).*|
| **2.** |Debido a un error en esta versión, podría ver instancias del código de error 110 en *error.xml* con nombres de elemento no reconocibles. | 
| **3.** |Debido a un error en esta versión, podría ver instancias del código de error 2003 durante la carga de archivos específicos. | 
| **4.** |En esta versión, puede actualizar solo un recurso compartido a la vez. | 


## <a name="known-issues-in-preview-release"></a>Problemas conocidos en la versión preliminar

En la tabla siguiente se proporciona un resumen de los problemas conocidos de la versión preliminar de Data Box Gateway.

|  No. | Característica | Problema | Soluciones alternativas o comentarios |
| --- | --- | --- | --- |
| **1.** |Actualizaciones |Los dispositivos de Data Box Gateway creados en las versiones preliminares anteriores se pueden actualizar a esta versión. |Descargar las imágenes de disco virtual de la nueva versión y configure e implemente nuevos dispositivos. Para más información, consulte [Preparación de la implementación de Azure Data Box Gateway](data-box-gateway-deploy-prep.md). |
| **2.** |Disco de datos aprovisionado |Después de aprovisionar un disco de datos de un determinado tamaño y de crear la correspondiente instancia de Data Box Gateway, no debe reducir el disco de datos. Si intenta hacer esto, se perderán todos los datos locales del dispositivo. | |
| **3.** |Cambiar nombre |No se permite cambiar el nombre de los objetos. |Si esta característica es fundamental para su flujo de trabajo, póngase en contacto con el Soporte técnico de Microsoft. |
| **4.** |Copiar| Si se copia un archivo de solo lectura en el dispositivo, no se conserva la propiedad de solo lectura. | |
| **5.** |Tipos de archivo | No se admiten los siguientes tipos de archivos de Linux: archivos de caracteres, archivos de bloqueo, sockets, canalizaciones y vínculos simbólicos.  |Al copiar estos archivos, se crean archivos de longitud 0 en el recurso compartido NFS. Estos archivos permanecen en estado de error y también se notifican en *error.xml*. |
| **6.** |Eliminación | Debido a un error en esta versión, si se elimina un recurso compartido NFS, es posible que el recurso compartido no se pueda eliminar. El estado del recurso compartido será *Deleting* (Eliminando).  |Esto solo se produce cuando el recurso compartido usa un nombre de archivo no admitido. |
| **7.** |Actualizar | En una operación de actualización, no se conservan los permisos y las listas de control de acceso (ACL).  | |
| **8.** |Copiar | La copia de datos produce un error con el mensaje Error:  no se pudo completar la operación solicitada por una limitación del sistema de archivos.  |Este error se produce cuando la secuencia de datos alternativa (ADS) asociada con el archivo supera los 128 KB (límite máximo de ReFS).  |
| **9.** |Vínculos simbólicos |No se admiten los vínculos simbólicos.  |Como resultado de los vínculos simbólicos a directorios, estos nunca se marcan como sin conexión. Por este motivo, no puede ver la cruz gris que indica que los directorios están disponibles sin conexión y que todo el contenido asociado se cargó por completo en Azure. |
| **10.** |Recursos compartidos |La actualización de un contenedor existente con blobs en páginas en un recurso compartido de blob en bloques (o viceversa) da lugar a errores de carga en la modificación de un archivo.  |Este comportamiento se puede observar al seguir estos pasos: <li> Crear un recurso compartido de blob en bloques en el dispositivo. </li><li> Asociar el recurso compartido a un contenedor en la nube existente que tiene blobs en páginas.</li><li>Actualizar ese recurso compartido. </li><li>Modificar algunos de los archivos actualizados que ya están almacenados como blobs en páginas en la nube.</li> Se observan errores de carga. |
| **11.** |Ayuda en línea |Los vínculos de Ayuda en Azure Portal podrían no conducir a la documentación.|Los vínculos de Ayuda funcionarán en la versión de disponibilidad general. |

## <a name="next-steps"></a>Pasos siguientes

- [Preparación de la implementación de Azure Data Box Gateway](data-box-gateway-deploy-prep.md)


