---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 05/21/2019
ms.author: alkohli
ms.openlocfilehash: b159ec8620fa8c93e4917f73be9b9898e1b4fbcc
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66244562"
---
- No copie los archivos directamente en cualquiera de los recursos compartidos creados previamente. Deberá crear una carpeta en el recurso compartido y, a continuación, copiar archivos en esa carpeta.
- Una carpeta en *StorageAccount_BlockBlob* y *StorageAccount_PageBlob* es un contenedor. Por ejemplo, los contenedores se crean como *StorageAccount_BlockBlob/container* y *StorageAccount_PageBlob/container*.
- Todas las carpetas que se han creado creó directamente en *StorageAccount_AzureFiles* se traducen en un recurso compartido de archivos de Azure.
- Si tiene un objeto existente de Azure (por ejemplo, un blob o un archivo) en la nube con el mismo nombre que el objeto que se está copiando, Data Box sobrescribirá el archivo en la nube.
- Todos los archivos escritos en los recursos compartidos *StorageAccount_BlockBlob* y *StorageAccount_BlockBlob* se cargan como blob en bloques y blob en páginas, respectivamente.
- Almacenamiento de blobs de Azure no es compatible con los directorios. Si crea una carpeta en la carpeta *StorageAccount_BlockBlob* y, después, se crean carpetas virtuales en el nombre del blob. Para Azure Files, se mantiene la estructura de directorios real.
- Cualquier vaciar una jerarquía de directorios (sin los archivos) que creó en *StorageAccount_BlockBlob* y *StorageAccount_PageBlob* carpetas no está cargado.
- Si se han producido errores al cargar datos en Azure, se crea un registro de errores en la cuenta de almacenamiento de destino. La ruta de acceso a este registro de errores está disponible cuando se completa la carga. Puede examinar el registro para realizar acciones correctivas. No elimine los datos desde el origen sin comprobar los datos cargados.
