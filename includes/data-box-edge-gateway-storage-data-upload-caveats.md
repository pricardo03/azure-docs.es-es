---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/21/2019
ms.author: alkohli
ms.openlocfilehash: 8c87e14071b3bb40421ab655c172df739570e295
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60755211"
---
Las advertencias siguientes se aplican a los datos cuando se trasladan en Azure.

- Se recomienda que no haya más de un dispositivo que escriba en el mismo contenedor.
- Si tiene un objeto existente de Azure (por ejemplo, un blob o un archivo) en la nube con el mismo nombre que el objeto que se está copiando, el dispositivo sobrescribirá el archivo en la nube.
- Una jerarquía de directorios vacía (sin archivos) que se creó en las carpetas de recurso compartido no se carga en los contenedores de blobs.
- Puede copiar los datos mediante el método arrastrar y colocar con el Explorador de archivos o a través de la línea de comandos. Si el tamaño total de archivos que se copia es mayor que 10 GB, se recomienda que usar un programa de copia masiva, como Robocopy o rsync. Las herramientas de copia masiva se vuelva a intentar la operación de copia para errores intermitentes y proporcionan resistencia adicional.
- Si el recurso compartido asociado al contenedor de almacenamiento de Azure carga los blobs que no coinciden con el tipo de blobs definido para el recurso compartido en el momento de creación, estos blobs no se actualizan. Por ejemplo, se crea un recurso compartido de blob en bloques en el dispositivo. Se asocia el recurso compartido a un contenedor en la nube existente que tiene blobs en páginas. Se actualiza ese recurso compartido para descargar los archivos. Se modifican algunos de los archivos actualizados que ya están almacenados como blobs en páginas en la nube. Aparecerán errores de carga.
