---
title: Actualización de los destinos de almacenamiento en Azure HPC Cache
description: Procedimientos para editar los destinos de almacenamiento en Azure HPC Cache
author: ekpgh
ms.service: hpc-cache
ms.topic: conceptual
ms.date: 11/4/2019
ms.author: rohogue
ms.openlocfilehash: 757fbc166687110b9061ab95e96b0182e0ad3774
ms.sourcegitcommit: 6c2c97445f5d44c5b5974a5beb51a8733b0c2be7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73622749"
---
# <a name="edit-storage-targets"></a>Edición de los destinos de almacenamiento

Se pueden editar los destinos de almacenamiento para modificar algunas de sus propiedades. Las distintas propiedades son editables para tipos de almacenamiento diferentes:

* En el caso de los destinos de almacenamiento de blob, se puede cambiar la ruta de acceso del espacio de nombres.

* En el caso de los destinos de almacenamiento de NFS, se pueden cambiar estas propiedades:

  * Ruta de acceso del espacio de nombres
  * Modelo de uso
  * Exportación
  * Subdirectorio de exportación

No se puede editar el nombre, el tipo o el sistema de almacenamiento de back-end de un destino de almacenamiento (contenedor de blobs, o dirección IP o nombre de host de NFS). Si tiene que cambiar estas propiedades, elimine el destino de almacenamiento y cree un reemplazo con el valor nuevo.

Para modificar un destino de almacenamiento, haga clic en el nombre del destino de almacenamiento con el fin de abrir su página de detalles. Algunos campos de la página son editables.

![captura de pantalla de la página de edición de un destino de almacenamiento de NFS](media/hpc-cache-edit-storage-nfs.png)

## <a name="update-an-nfs-storage-target"></a>Actualización de un destino de almacenamiento de NFS

En el caso de un destino de almacenamiento de NFS, se pueden actualizar varias propiedades. (Consulte la captura de pantalla anterior para obtener una página de edición de ejemplo).

* **Modelo de uso**: el modelo de uso influye en el modo en que la memoria caché conserva los datos. Lea [Elección de un modelo de uso](hpc-cache-add-storage.md#choose-a-usage-model) para obtener más información.
* **Ruta de acceso del espacio de nombres virtual**: ruta de acceso que los clientes usan para montar este destino de almacenamiento. Lea [Planeamiento del espacio de nombres agregado](hpc-cache-namespace.md) para obtener más detalles.
* **Ruta de acceso de exportación de NFS**: exportación del sistema de almacenamiento que se va a usar para esta ruta de acceso del espacio de nombres.
* **Ruta de acceso del subdirectorio**: subdirectorio (en la exportación) que se va a asociar a esta ruta de acceso del espacio de nombres. Deje este campo en blanco si no necesita especificar un subdirectorio.

Cada ruta de acceso del espacio de nombres necesita una combinación única de exportación y subdirectorio. Es decir, no se pueden crear dos rutas de acceso de cliente distintas al mismo directorio en el sistema de almacenamiento de back-end.

Después de realizar los cambios, haga clic en **Aceptar** para actualizar el destino de almacenamiento o en **Cancelar** para descartar los cambios.

## <a name="update-an-azure-blob-storage-target"></a>Actualización de un destino de almacenamiento de blobs de Azure

La página de detalles de un destino de almacenamiento de blob permite modificar la ruta de acceso del espacio de nombres virtual. 

![captura de pantalla de la página de edición de un destino de almacenamiento de blob](media/hpc-cache-edit-storage-blob.png)

Cuando se haya terminado, haga clic en **Aceptar** para actualizar el destino de almacenamiento o en **Cancelar** para descartar los cambios.

## <a name="next-steps"></a>Pasos siguientes

* Lea [Incorporación de destinos de almacenamiento](hpc-cache-add-storage.md) para obtener más información sobre estas opciones.
* Lea [Planeamiento del espacio de nombres agregado](hpc-cache-namespace.md) para obtener más recomendaciones sobre el uso de rutas de acceso virtuales.
