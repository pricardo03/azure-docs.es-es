---
title: Use Data Box y otros métodos para la ingesta de datos sin conexión en Azure File Sync.
description: Proceso y procedimientos recomendados para que la migración masiva sea compatible con la sincronización.
services: storage
author: fauhse
ms.service: storage
ms.topic: article
ms.date: 02/12/2019
ms.author: fauhse
ms.component: files
ms.openlocfilehash: a184e0563d1ad26671c38cabe07f42d97cbe2885
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56212800"
---
# <a name="migrate-to-azure-file-sync"></a>Migración a Azure File Sync
Hay varias opciones para mover los archivos a Azure File Sync:

### <a name="uploading-files-via-azure-file-sync"></a>Carga de archivos a través de Azure File Sync
La opción más sencilla consiste en mover los archivos localmente a un servidor de Windows Server 2012 R2 o posterior e instalar al agente de Azure File Sync. Una vez configurada la sincronización, los archivos se cargarán desde el servidor. Actualmente, los clientes están observando una velocidad de carga promedio de 1 TB aproximadamente cada 2 días.
Considere la posibilidad de aplicar una [programación de limitación de ancho de banda](storage-sync-files-server-registration.md#ensuring-azure-file-sync-is-a-good-neighbor-in-your-datacenter) para asegurarse de que el comportamiento del servidor en el centro de datos es el adecuado.

### <a name="offline-bulk-transfer"></a>Transferencias masivas sin conexión
Aunque la carga es realmente la opción más sencilla, puede no funcionar automáticamente si el ancho de banda disponible no permite sincronizar los archivos en Azure en un período razonable. La dificultad que hay superar aquí es la sincronización inicial de todo el conjunto de archivos. Posteriormente, Azure File Sync solo moverá los cambios cuando se producen en el espacio de nombres, que normalmente utiliza mucho menos ancho de banda.
Para superar esta dificultad de carga inicial, se pueden usar las herramientas de migración masiva sin conexión, como la [familia de Azure Data Box](https://azure.microsoft.com/services/storage/databox). El siguiente artículo se centra en el proceso que debe seguir para beneficiarse de la migración sin conexión de una forma compatible con Azure File Sync. Describiremos los procedimientos recomendados que lo ayudarán a evitar los archivos conflictivos y a conservar las ACL de archivos y carpetas y las marcas de tiempo una vez que habilite la sincronización.

### <a name="online-migration-tools"></a>Herramientas de migración en línea
El procedimiento descrito a continuación no solo funciona con Data Box. También lo hace con cualquier herramienta de migración sin conexión (por ejemplo, Data Box) o en línea, como AzCopy, Robocopy o servicios y herramientas de terceros. Por tanto, independientemente del método para superar la dificultad de la carga inicial, es importante seguir los pasos descritos a continuación para utilizar estas herramientas de una forma compatible con la sincronización.


## <a name="offline-data-transfer-benefits"></a>Ventajas de la transferencia de datos sin conexión
Las principales ventajas de la migración sin conexión al usar Data Box son las siguientes:

- Al migrar archivos a Azure a través de un proceso de transferencia masiva sin conexión, como Data Box, no tendrá que cargar todos los archivos desde el servidor a través de la red. En los espacios de nombres de gran tamaño, esto podría suponer un ahorro considerable de tiempo y ancho de banda de red.
- Si usa Azure File Sync, independientemente del modo de transporte utilizado (Data Box, Azure Import, etc.), el servidor activo solo carga los archivos que han cambiado desde que se enviaron los datos a Azure.
- Azure File Sync garantiza que las ACL de archivos y carpetas se sincronizan también, aunque el producto de migración masiva sin conexión no transporte ACL.
- Al usar Azure Data Box y Azure File Sync, no hay tiempo de inactividad. Si se utiliza Data Box para transferir datos a Azure, el ancho de banda de red se aprovecha de forma eficaz al tiempo que se conserva la fidelidad de los archivos. También se mantiene el espacio de nombres actualizado cargando solo los archivos que han cambiado desde que se envió el dispositivo Data Box.

## <a name="plan-your-offline-data-transfer"></a>Planeación de la transferencia de datos sin conexión
Antes de comenzar, revise la siguiente información:

- Complete la migración masiva a uno o varios recursos compartidos de archivos de Azure antes de habilitar la sincronización con Azure File Sync.
- Si planea usar Data Box para la migración masiva, siga estos pasos: Revise [los requisitos previos de implementación de Data Box](../../databox/data-box-deploy-ordered.md#prerequisites).
- Planee la topología de Azure File Sync final: [Planeamiento de una implementación de Azure Files Sync](storage-sync-files-planning.md)
- Seleccione las cuentas de almacenamiento de Azure que contendrán los recursos compartidos de archivos con los que desea sincronizarlas. Asegúrese de que la migración masiva a recursos compartidos de almacenamiento provisionales temporales se realiza en las mismas cuentas de almacenamiento. La migración masiva solo puede habilitarse mediante un recurso compartido final de almacenamiento provisional que resida en la misma cuenta de almacenamiento.
- Solo se puede usar una migración masiva cuando se crea una nueva relación de sincronización con una ubicación de servidor. No se puede habilitar una migración masiva con una relación de sincronización existente.

## <a name="offline-data-transfer-process"></a>Transferencia de datos sin conexión
En esta sección se describe el proceso de configuración de Azure File Sync de una forma compatible con las herramientas de migración masiva, como Azure Data Box.

![Visualización de los pasos del proceso que también se explican con detalle en el siguiente párrafo](media/storage-sync-files-offline-data-transfer/data-box-integration-1-600.png)

| Paso | Detalles |
|---|---------------------------------------------------------------------------------------|
| ![Paso 1 del proceso](media/storage-sync-files-offline-data-transfer/bullet_1.png) | [Pida su Data Box](../../databox/data-box-deploy-ordered.md). Hay [varias ofertas dentro de la familia de Data Box](https://azure.microsoft.com/services/storage/databox/data) para satisfacer sus necesidades. Reciba su dispositivo Data Box y siga su [documentación para copiar los datos](../../databox/data-box-deploy-copy-data.md#copy-data-to-data-box). Asegúrese de que los datos se copian en esta ruta de acceso UNC en el dispositivo Data Box: `\\<DeviceIPAddres>\<StorageAccountName_AzFile>\<ShareName>`, donde `ShareName` es el nombre del recurso compartido de almacenamiento provisional. Envíe el dispositivo Data Box a Azure. |
| ![Paso 2 del proceso](media/storage-sync-files-offline-data-transfer/bullet_2.png) | Espere hasta que los archivos se muestren en los recursos compartidos de archivos de Azure que haya designado como recursos compartidos de almacenamiento provisionales temporales. **No habilite la sincronización con estos recursos compartidos.** |
| ![Paso 3 del proceso](media/storage-sync-files-offline-data-transfer/bullet_3.png) | Cree un recurso compartido vacío para cada recurso compartido de archivos que Data Box creó automáticamente. Asegúrese de que este nuevo recurso compartido esté en la misma cuenta de almacenamiento que el recurso compartido de Data Box. [Creación de un recurso compartido de archivos de Azure](storage-how-to-create-file-share.md). |
| ![Paso 4 del proceso](media/storage-sync-files-offline-data-transfer/bullet_4.png) | [Cree un grupo de sincronización](storage-sync-files-deployment-guide.md#create-a-sync-group-and-a-cloud-endpoint) en un servicio de sincronización de almacenamiento y haga referencia el recurso compartido vacío como un punto de conexión en la nube. Repita este paso para cada recurso compartido de archivos de Data Box. Revise la guía de [implementación de Azure File Sync](storage-sync-files-deployment-guide.md) y siga los pasos necesarios para configurar Azure File Sync. |
| ![Paso 5 del proceso](media/storage-sync-files-offline-data-transfer/bullet_5.png) | [Agregue el directorio de servidor activo como un punto de conexión de servidor](storage-sync-files-deployment-guide.md#create-a-server-endpoint). Especifique en el proceso que ya se ha movido los archivos a Azure y haga referencia a los recursos compartidos de almacenamiento provisionales. Tendrá que decidir si habilitar o deshabilitar la nube por niveles según sea necesario. Al crear un punto de conexión de servidor en el servidor activo, debe hacer referencia al recurso compartido de almacenamiento provisional. Habilite "Offline Data Transfer" (Transferencia de datos sin conexión) (imagen siguiente) en la hoja New server endpoint (Nuevo punto de conexión de servidor) y haga referencia al recurso compartido de almacenamiento provisional que deberá residir en la misma cuenta de almacenamiento que el punto de conexión en la nube. La lista de recursos compartidos disponibles se filtra por cuenta de almacenamiento y recursos compartidos que ya no se están sincronizando. |

![Visualización de la interfaz de usuario de Azure Portal para habilitar Offline Data Transfer (Transferencia de datos sin conexión) durante la creación de un nuevo punto de conexión de servidor.](media/storage-sync-files-offline-data-transfer/data-box-integration-2-600.png)

## <a name="syncing-the-share"></a>Sincronización del recurso compartido
Una vez haya creado el punto de conexión de servidor, se iniciará la sincronización. Para cada archivo del servidor, la sincronización determinará si el archivo también se encuentra en el recurso compartido de almacenamiento provisional donde Data Box depositó los archivos y, por lo tanto, la sincronización copiará el archivo desde el recurso compartido de almacenamiento provisional, en lugar de cargarlo desde el servidor. Si el archivo no está en el recurso compartido de almacenamiento provisional o hay una versión más reciente en el servidor local, la sincronización cargará el archivo desde el servidor local.

> [!IMPORTANT]
> Solo se puede habilitar el modo de migración masiva durante la creación de un punto de conexión de servidor. Una vez que se ha establecido un punto de conexión de servidor, no hay ninguna manera de integrar los datos migrados de forma masiva desde un servidor que ya está sincronizándose en el espacio de nombres.

## <a name="acls-and-timestamps-on-files-and-folders"></a>ACL y marcas de tiempo en archivos y carpetas
Azure File Sync se asegurará de que las ACL de archivos y carpetas se sincronizan desde el servidor activo, aunque la herramienta de migración masiva que se usara no transportara ACL inicialmente. Es decir, no pasa nada si el recurso compartido de almacenamiento provisional no contiene ningún ACL en archivos y carpetas. Al habilitar la característica de migración de datos sin conexión cuando crea un punto de conexión de servidor, las ACL se sincronizarán en ese momento para todos los archivos del servidor. Lo mismo sucede con las marcas de tiempo de creación y modificación.

## <a name="shape-of-the-namespace"></a>Forma del espacio de nombres
La forma del espacio de nombres viene determinada por lo que aparece en el servidor cuando se habilita la sincronización. Si se eliminan archivos del servidor local después de los procesos de migración y creación de instantáneas de Data Box, estos archivos no se colocarán en el espacio de nombres activo que se está sincronizando. Seguirán estando en el recurso compartido de almacenamiento provisional, pero no se copiarán nunca. Este es el comportamiento deseado, ya que la sincronización conserva el espacio de nombres según el servidor activo. La "instantánea" de Data Box es simplemente un espacio de almacenamiento provisional para copiar archivos de forma eficaz, y no la autoridad pe la forma del espacio de nombres activo.

## <a name="finishing-bulk-migration-and-clean-up"></a>Finalización y limpieza de la migración masiva
En la captura de pantalla siguiente se muestra la hoja de propiedades del punto de conexión de servidor en Azure Portal. En la sección Offline Data Transfer (Transferencia de datos sin conexión), puede observar el estado del proceso. Mostrará "En curso" o "Completado".

Después de que el servidor complete la sincronización inicial de todo el espacio de nombres, habrá terminado de aprovechar el recurso compartido de almacenamiento provisional con los archivos migrados de forma masiva de Data Box. Observe en las propiedades del punto de conexión de servidor para la transferencia de datos sin conexión que el estado cambia a "Completado". En este momento, puede limpiar el recurso compartido de almacenamiento provisional para ahorrar costos:

1. Haga clic en "Disable offline data transfer" (Deshabilitar transferencia de datos sin conexión) en las propiedades del punto de conexión de servidor, cuando el estado sea "Completado".
2. Considere la posibilidad de eliminar el recurso compartido de almacenamiento provisional para ahorrar costos. Es poco probable que el recurso compartido de almacenamiento provisional contenga ACL de archivos y carpetas, y por lo tanto, es de uso limitado. Con fines de realizar copias de seguridad a un momento dado, cree una [instantánea real del recurso compartido de archivos de Azure que se está sincronizando](storage-snapshots-files.md). También puede [habilitar Azure Backup para realizar instantáneas]( ../../backup/backup-azure-files.md) según una programación.

![Visualización de la interfaz de usuario de Azure Portal de las propiedades del punto de conexión del servidor donde se encuentran los controles de estado y deshabilitación para la transferencia de datos sin conexión.](media/storage-sync-files-offline-data-transfer/data-box-integration-3-444.png)

Solo debe deshabilitar este modo cuando el estado sea "Completado" o si realmente desea anularlo debido a una configuración incorrecta. Si está desactivando el modo a medio camino de una implementación legítima, los archivos comenzarán a cargarse desde el servidor, aunque el recurso compartido de almacenamiento provisional siga estando disponible.

> [!IMPORTANT]
> Después de deshabilitar la transferencia de datos sin conexión no hay forma de volver a habilitarla, incluso si el recurso compartido de almacenamiento provisional de la migración masiva sigue estando disponible.

## <a name="next-steps"></a>Pasos siguientes
- [Planeamiento de una implementación de Azure File Sync](storage-sync-files-planning.md)
- [Implementación de Azure File Sync](storage-sync-files-deployment-guide.md)
