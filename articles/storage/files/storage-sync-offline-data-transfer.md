---
title: Migrar datos a Azure File Sync con Azure Data Box y otros métodos
description: Migrar datos de forma masiva en una forma que sea compatible con Azure File Sync.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 02/12/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: d1ec5168b898d0aa75c12e6eb435e20c09de1929
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64700269"
---
# <a name="migrate-bulk-data-to-azure-file-sync"></a>Migrar datos de forma masiva a Azure File Sync
Puede migrar datos de forma masiva a Azure File Sync de dos maneras:

* **Cargue los archivos mediante el uso de Azure File Sync.** Este es el método más sencillo. Mover los archivos de forma local en Windows Server 2012 R2 o posterior e instalar al agente de Azure File Sync. Después de configurar la sincronización, se cargarán los archivos desde el servidor. (Actualmente, nuestros clientes obtienen una velocidad de carga promedio de 1 TiB sobre cada dos días.) Para asegurarse de que el servidor no usa mucho el ancho de banda del centro de datos, es posible que desee configurar un [programación de ancho de banda](storage-sync-files-server-registration.md#ensuring-azure-file-sync-is-a-good-neighbor-in-your-datacenter).
* **Transferir los archivos sin conexión.** Si no tiene suficiente ancho de banda, es posible que no pueda cargar archivos a Azure en un período de tiempo razonable. El desafío es la sincronización inicial de todo el conjunto de archivos. Para superar este desafío, usar las herramientas de migración masiva sin conexión, como el [familia de Azure Data Box](https://azure.microsoft.com/services/storage/databox). 

En este artículo se explica cómo migrar archivos sin conexión de forma que sea compatible con Azure File Sync. Siga estas instrucciones para evitar conflictos de archivos y conservar sus archivos y listas de control de acceso (ACL) de carpetas y las marcas de tiempo después de habilitar la sincronización.

## <a name="online-migration-tools"></a>Herramientas de migración en línea
El proceso se describe en este artículo de funciona no solo para el cuadro de datos, sino también para otras herramientas de migración sin conexión. También funciona para las herramientas en línea como AzCopy, Robocopy, o herramientas de asociados y servicios. Sin embargo superar la inicial carga desafío, siga los pasos descritos en este artículo para usar estas herramientas de forma que sea compatible con Azure File Sync.


## <a name="benefits-of-using-a-tool-to-transfer-data-offline"></a>Ventajas de usar una herramienta de transferencia de datos sin conexión
Estas son las principales ventajas de usar una herramienta de transferencia como cuadro de datos para la migración sin conexión:

- No tendrá que cargar todos los archivos a través de la red. Espacios de nombres de gran tamaño, esta herramienta puede ahorrar tiempo y ancho de banda de red significativo.
- Cuando se usa Azure File Sync, no importa qué herramienta de transferencia que utilice (servicio Azure Import/Export, Data Box etc.), el servidor activo carga solo los archivos que cambian después de mover los datos en Azure.
- Azure File Sync sincroniza las ACL de archivos y carpetas, incluso si la herramienta de migración masiva sin conexión no las ACL de transporte.
- Cuadro de datos y Azure File Sync no requieren ningún tiempo de inactividad. Cuando se usa para transferir datos a Azure Data Box, usa eficazmente el ancho de banda de red y conserva la fidelidad del archivo. También mantener su espacio de nombres actualizados cargando solo los archivos que cambian después de mover los datos en Azure.

## <a name="prerequisites-for-the-offline-data-transfer"></a>Requisitos previos para la transferencia de datos sin conexión
No se debe habilitar la sincronización en el servidor que se va a migrar antes de completar a la transferencia de datos sin conexión. Otras cosas a tener en cuenta antes de comenzar son los siguientes:

- Si planea usar Data Box para la migración masiva, siga estos pasos: Revise [los requisitos previos de implementación de Data Box](../../databox/data-box-deploy-ordered.md#prerequisites).
- Planear la topología de Azure File Sync final: [Planeamiento de una implementación de Azure Files Sync](storage-sync-files-planning.md)
- Seleccione las cuentas de almacenamiento de Azure que contendrán los recursos compartidos de archivos con los que desea sincronizarlas. Asegúrese de que la migración masiva a recursos compartidos de almacenamiento provisionales temporales se realiza en las mismas cuentas de almacenamiento. La migración masiva solo puede habilitarse mediante un recurso compartido final de almacenamiento provisional que resida en la misma cuenta de almacenamiento.
- Solo se puede usar una migración masiva cuando se crea una nueva relación de sincronización con una ubicación de servidor. No se puede habilitar una migración masiva con una relación de sincronización existente.


## <a name="process-for-offline-data-transfer"></a>Proceso de transferencia de datos sin conexión
Aquí le mostramos cómo configurar Azure File Sync de forma que sea compatible con las herramientas de migración masiva, como Azure Data Box:

![Diagrama que muestra cómo configurar Azure File Sync](media/storage-sync-files-offline-data-transfer/data-box-integration-1-600.png)

| Paso | Detalles |
|---|---------------------------------------------------------------------------------------|
| ![Paso 1](media/storage-sync-files-offline-data-transfer/bullet_1.png) | [Pida su Data Box](../../databox/data-box-deploy-ordered.md). Las ofertas de familia de Data Box [varios productos](https://azure.microsoft.com/services/storage/databox/data) para satisfacer sus necesidades. Cuando aparezca el cuadro de datos, siga su [documentación para copiar los datos](../../databox/data-box-deploy-copy-data.md#copy-data-to-data-box) a esta ruta de acceso UNC en el cuadro de datos:  *\\< DeviceIPAddres\>\<StorageAccountName_AzFile\> \<ShareName\>*. En este caso, *ShareName* es el nombre del recurso compartido de almacenamiento provisional. Envíe el dispositivo Data Box a Azure. |
| ![Paso 2](media/storage-sync-files-offline-data-transfer/bullet_2.png) | Espere hasta que los archivos se muestran en los recursos compartidos de archivos de Azure que eligió como recursos compartidos de almacenamiento provisionales temporales. *No habilite la sincronización para estos recursos compartidos.* |
| ![Paso 3](media/storage-sync-files-offline-data-transfer/bullet_3.png) | Cree un nuevo recurso compartido vacío para cada recurso compartido de archivos que Data Box se creó para usted. Este nuevo recurso compartido debe estar en la misma cuenta de almacenamiento que el recurso compartido de Data Box. [Creación de un recurso compartido de archivos de Azure](storage-how-to-create-file-share.md). |
| ![Paso 4](media/storage-sync-files-offline-data-transfer/bullet_4.png) | [Crear un grupo de sincronización](storage-sync-files-deployment-guide.md#create-a-sync-group-and-a-cloud-endpoint) en un servicio de sincronización de almacenamiento. Referencia al recurso de compartido vacía como un punto de conexión en la nube. Repita este paso para cada recurso compartido de archivos de Data Box. [Configurar Azure File Sync](storage-sync-files-deployment-guide.md). |
| ![Paso 5](media/storage-sync-files-offline-data-transfer/bullet_5.png) | [Agregue el directorio de servidor activo como un punto de conexión de servidor](storage-sync-files-deployment-guide.md#create-a-server-endpoint). En el proceso, especifica que mueva los archivos a Azure y hacer referencia a los recursos compartidos de almacenamiento provisionales. Puede habilitar o deshabilitar niveles según sea necesario de nube. Al crear un punto de conexión de servidor en el servidor en directo, hacer referencia el recurso compartido de almacenamiento provisional. En el **agregar punto de conexión de servidor** hoja, en **transferencia de datos sin conexión**, seleccione **habilitado**y, a continuación, seleccione el recurso compartido de almacenamiento provisional que debe estar en la misma cuenta de almacenamiento que la nube punto de conexión. En este caso, la lista de recursos compartidos disponibles se filtra por cuenta de almacenamiento y recursos compartidos que ya no se están sincronizando. |

![Captura de pantalla de la interfaz de usuario de Azure portal, que muestra cómo habilitar la transferencia de datos sin conexión durante la creación de un nuevo punto de conexión de servidor](media/storage-sync-files-offline-data-transfer/data-box-integration-2-600.png)

## <a name="syncing-the-share"></a>Sincronización del recurso compartido
Después de crear el punto de conexión de servidor, se iniciará la sincronización. El proceso de sincronización determina si cada archivo en el servidor también existe en el recurso compartido de almacenamiento provisional donde Data Box deposita los archivos. Si el archivo existe, el proceso de sincronización copia el archivo desde el recurso compartido de almacenamiento provisional en lugar de cargarlos desde el servidor. Si el archivo no existe en el recurso compartido de almacenamiento provisional, o una versión más reciente está disponible en el servidor local, el proceso de sincronización carga el archivo desde el servidor local.

> [!IMPORTANT]
> Puede habilitar el modo de migración masiva solo al crear un punto de conexión de servidor. Después de establecer un punto de conexión de servidor, no se puede integrar migrar de forma masiva datos desde un servidor de sincronización ya en el espacio de nombres.

## <a name="acls-and-timestamps-on-files-and-folders"></a>ACL y marcas de tiempo en archivos y carpetas
Azure File Sync, se garantiza que las ACL de archivos y carpetas se sincronizan desde el servidor activo incluso si la herramienta de migración masiva que se usó inicialmente no las ACL de transporte. Por este motivo, el recurso compartido de almacenamiento provisional no tiene que contener las ACL de archivos y carpetas. Cuando se habilita la característica de migración de datos sin conexión a medida que cree un nuevo punto de conexión de servidor, se sincronizan todas las ACL del archivo en el servidor. También se sincronizan las marcas de tiempo recién creadas y modificadas.

## <a name="shape-of-the-namespace"></a>Forma del espacio de nombres
Cuando se habilita la sincronización, el contenido del servidor determina la forma del espacio de nombres. Si después de fin la instantánea del cuadro de datos y la migración, estos archivos no se mueven al espacio de nombres en vivo, sincronización, se eliminan archivos desde el servidor local. Permanecen en el recurso compartido de almacenamiento provisional, pero no se copian. Esto es necesario porque la sincronización mantiene el espacio de nombres según el servidor activo. Data Box *instantánea* es simplemente un almacenamiento provisional para copia de archivos eficaz. No es la autoridad para la forma del espacio de nombres en vivo.

## <a name="cleaning-up-after-bulk-migration"></a>Limpiar después de la migración masiva 
Como el servidor finaliza la sincronización inicial del espacio de nombres, los archivos de migrar de forma masiva de Data Box usan el recurso compartido de archivos de almacenamiento provisional. En el **propiedades del punto de conexión de servidor** hoja en Azure portal, en la **transferencia de datos sin conexión** sección, el estado cambia de **en curso** a **completado** . 

![Captura de pantalla de la hoja de propiedades del punto de conexión de servidor, donde se encuentran los controles de estado y deshabilitar la transferencia de datos sin conexión](media/storage-sync-files-offline-data-transfer/data-box-integration-3-444.png)

Ahora puede limpiar el recurso compartido de almacenamiento provisional para ahorrar costos:

1. En el **propiedades del punto de conexión de servidor** hoja, cuando el estado es **completado**, seleccione **deshabilitar la transferencia de datos sin conexión**.
2. Considere la posibilidad de eliminar el recurso compartido de almacenamiento provisional para ahorrar costos. El recurso compartido de almacenamiento provisional probablemente no contiene las ACL de archivos y carpetas, por lo que no es muy útil. Para fines de copia de seguridad en un momento, cree un número real [instantánea del recurso compartido de archivos de Azure sincronización](storage-snapshots-files.md). También puede [configurar copias de seguridad de Azure para tomar instantáneas]( ../../backup/backup-azure-files.md) según una programación.

Deshabilitar el modo de transferencia de datos sin conexión solo cuando el estado es **completado** o cuando desee cancelar debido a una configuración incorrecta. Si deshabilita el modo durante la implementación, los archivos empieza a cargar desde el servidor, incluso si el recurso compartido de almacenamiento provisional sigue estando disponible.

> [!IMPORTANT]
> Después de deshabilitar el modo de transferencia de datos sin conexión, no puede habilitarlo de nuevo, incluso si el recurso compartido de almacenamiento provisional de la migración masiva sigue estando disponible.

## <a name="next-steps"></a>Pasos siguientes
- [Planeamiento de una implementación de Azure Files Sync](storage-sync-files-planning.md)
- [Implementación de Azure File Sync](storage-sync-files-deployment-guide.md)
