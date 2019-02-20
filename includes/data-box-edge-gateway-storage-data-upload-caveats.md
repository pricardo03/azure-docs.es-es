---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 02/07/2019
ms.author: alkohli
ms.openlocfilehash: ad7ceffed61665a729d4391b5f0ff2935375c253
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2019
ms.locfileid: "55966937"
---
Las advertencias siguientes se aplican a los datos cuando se trasladan en Azure.

- Se recomienda que no haya más de un dispositivo que escriba en el mismo contenedor.
- Si tiene un objeto existente de Azure (por ejemplo, un blob o un archivo) en la nube con el mismo nombre que el objeto que se está copiando, el dispositivo sobrescribirá el archivo en la nube.
- Una jerarquía de directorios vacía (sin archivos) que se creó en las carpetas de recurso compartido no se carga en los contenedores de blobs.
- Para archivos de gran tamaño, se recomienda usar robocopy, ya que reintenta la operación de copia para errores intermitentes.
