---
title: Migración de recursos de almacenamiento de Azure desde Azure Alemania a Azure global
description: En este artículo se proporciona información sobre cómo migrar los recursos de almacenamiento de Azure desde Azure Alemania a Azure global.
author: gitralf
services: germany
cloud: Azure Germany
ms.author: ralfwi
ms.service: germany
ms.date: 08/15/2018
ms.topic: article
ms.custom: bfmigrate
ms.openlocfilehash: db26380ef50fd7546f90ceccaec854587e4690c4
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67033521"
---
# <a name="migrate-storage-resources-to-global-azure"></a>Migración de recursos de almacenamiento a Azure global

En este artículo se proporciona información que puede ayudarle a migrar los recursos de almacenamiento de Azure desde Azure Alemania a Azure global.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="blobs"></a>Blobs

AzCopy es una herramienta gratuita que puede usar para copiar blobs, archivos y tablas. AzCopy se puede usar en las migraciones de Azure a Azure, de un entorno local a Azure y de Azure a un entorno local. Use AzCopy en la migración para copiar blobs directamente desde Azure Germany a Azure global.

Si no usa discos administrados en la máquina virtual de origen, utilice AzCopy para copiar los archivos .vhd al entorno de destino. En caso contrario, debe realizar algunos pasos antes. Para más información, consulte [Recomendaciones para discos administrados](#managed-disks).

En el ejemplo siguiente se muestra el funcionamiento de AzCopy. Para tener una referencia completa, consulte la [documentación de AzCopy](../storage/common/storage-use-azcopy.md).

AzCopy utiliza los términos **Origen** y **Dest.** , expresados en forma de identificadores URI. Los identificadores URI de Azure Alemania siempre tienen este formato:

```http
https://<storageaccountname>.blob.core.cloudapi.de/<containername>/<blobname>
```

Los identificadores URI de Azure global siempre tienen este formato:

```http
https://<storageaccountname>.blob.core.windows.net/<containername>/<blobname>
```

Las tres partes del identificador URI (*storageaccountname*, *containername* y *blobname*) se obtienen en el portal, mediante PowerShell o mediante la CLI de Azure. El nombre del blob puede formar parte del identificador URI o puede presentarse como un patrón, como *vm121314.vhd*.

También se necesitan las claves de la cuenta de almacenamiento para acceder a la cuenta de Azure Storage. Se pueden obtener del portal, mediante PowerShell o mediante la CLI. Por ejemplo:

```powershell
Get-AzStorageAccountKey -Name <saname> -ResourceGroupName <rgname>
```

Como siempre, solo se necesita una de las dos claves para cada cuenta de almacenamiento.

Ejemplo:

Parte de identificador URI | Valor de ejemplo
-------- | --------------
Source storageAccount | `migratetest`
Source container | `vhds`
Source blob | `vm-121314.vhd`
Target storageAccount | `migratetarget`
Target container | `targetcontainer`

Este comando copia un disco duro virtual de Azure Alemania en Azure global (las claves se han acortado para mejorar la legibilidad):

```cmd
azcopy -v /source:https://migratetest.blob.core.cloudapi.de/vhds /sourcekey:"0LN...w==" /dest:https://migratetarget.blob.core.windows.net/targetcontainer /DestKey:"o//ucDi5TN...w==" /Pattern:vm-121314.vhd
```

Para obtener una copia coherente del disco duro virtual, apague la máquina virtual antes de copiarlo. Planee un tiempo de inactividad para la actividad de copia. Cuando se copie el disco duro virtual, [recompile la máquina virtual en el entorno de destino](../backup/backup-azure-vms-automation.md#create-a-vm-from-restored-disks).

Para obtener más información:

- Consulte la [documentación de AZCopy](../storage/common/storage-use-azcopy.md).
- Aprenda a [crear una máquina virtual a partir de discos restaurados](../backup/backup-azure-vms-automation.md#create-a-vm-from-restored-disks).

## <a name="managed-disks"></a>Managed Disks

Azure Managed Disks simplifica la administración de discos en las máquinas virtuales de Azure IaaS (infraestructura como servicio), ya que administra las cuentas de almacenamiento asociadas al discos de la máquina virtual. 

Al no tener acceso directo al archivo .vhd, directamente no se pueden usar directamente herramientas como AzCopy para copiar los archivos (consulte [Blobs](#blobs)). La solución consiste en exportar primero el disco administrado mediante la obtención de un identificador URI de firma de acceso compartido temporal y, después, descargarlo o copiarlo con esta información. Las secciones siguientes muestran un ejemplo de cómo obtener el identificador URI de firma de acceso compartido y qué hacer con él.

### <a name="step-1-get-the-shared-access-signature-uri"></a>Paso 1: Obtención del identificador URI de firma de acceso compartido

1. En el portal, busque su disco administrado. (Está en el mismo grupo de recursos que la máquina virtual. El tipo de recurso es **Disco**.)
1. En la página **Información general**, seleccione el botón **Exportar** en el menú superior (antes debe apagar la máquina virtual y desasignarla, o bien desasociarla).
1. Defina la hora a la que desea que expire el identificador URI (el valor predeterminado es 3600 segundos).
1. Genere una dirección URL (este paso debería tardar solo unos segundos).
1. Copie la dirección URL (aparece una sola vez).

### <a name="step-2-azcopy"></a>Paso 2: AzCopy

Para obtener ejemplos de cómo utilizar AzCopy, consulte [Blobs](#blobs). Use AzCopy (u otra herramienta similar) para copiar el disco directamente desde el entorno de origen al de destino. En AzCopy, tiene que dividir el identificador URI en identificador URI base y la parte de la firma de acceso compartido. La parte de la firma de acceso compartido del identificador URI comienza con el carácter " **?** ". El portal proporciona este identificador URI como identificador URI de firma de acceso compartido:

```http
https://md-kp4qvrzhj4j5.blob.core.cloudapi.de/r0pmw4z3vk1g/abcd?sv=2017-04-17&sr=b&si=22970153-4c56-47c0-8cbb-156a24b6e4b5&sig=5Hfu0qMw9rkZf6mCjuCE4VMV6W3IR8FXQSY1viji9bg%3D>
```

Los comandos siguientes muestran los parámetros de origen de AzCopy:

```cmd
/source:"https://md-kp4qvrzhj4j5.blob.core.cloudapi.de/r0pmw4z3vk1g/abcd"
```

```cmd
/sourceSAS:" ?sv=2017-04-17&sr=b&si=22970153-4c56-47c0-8cbb-156a24b6e4b5&sig=5Hfu0qMw9rkZf6mCjuCE4VMV6W3IR8FXQSY1viji9bg%3D"
```

Este es el comando completo:

```cmd
azcopy -v /source:"https://md-kp4qvrzhj4j5.blob.core.cloudapi.de/r0pmw4z3vk1g/abcd" /sourceSAS:"?sv=2017-04-17&sr=b&si=22970153-4c56-47c0-8cbb-156a24b6e4b5&sig=5Hfu0qMw9rkZf6mCjuCE4VMV6W3IR8FXQSY1viji9bg%3D" /dest:"https://migratetarget.blob.core.windows.net/targetcontainer/newdisk.vhd" /DestKey:"o//ucD... Kdpw=="
```

### <a name="step-3-create-a-new-managed-disk-in-the-target-environment"></a>Paso 3: Creación de un disco administrado en el entorno de destino

Hay varias opciones para crear discos administrados. Así es como se hace en Azure Portal:

1. En el portal, seleccione **Nuevo** > **Disco administrado** > **Crear**.
1. Escriba un nombre para el disco nuevo.
1. Seleccione un grupo de recursos.
1. En **Tipo de origen**, seleccione **Storage Blob**. A continuación, copie el identificador URI de destino del comando AzCopy o examine para seleccionar el identificador URI de destino.
1. Si ha copiado un disco del sistema operativo, seleccione el tipo de **sistema operativo**. Para otros tipos de disco, seleccione **Crear**.

### <a name="step-4-create-the-vm"></a>Paso 4: Creación de la máquina virtual

Como ya se ha indicado, hay varias formas de crear una máquina virtual mediante el uso de este nuevo disco administrado. Estas son dos posibles opciones:

- En el portal, seleccione el disco y, después, seleccione **Crear VM**. Defina los restantes parámetros de la máquina virtual de la forma habitual.
- Para PowerShell, consulte [Creación de una máquina virtual a partir de discos restaurados](../backup/backup-azure-vms-automation.md#create-a-vm-from-restored-disks).

Para obtener más información:

- Aprenda a exportar a disco [a través de API](/rest/api/compute/disks/grantaccess) obteniendo un identificador URI de firma de acceso compartido. 
- Aprenda a crear un disco administrado [a través de API](/rest/api/compute/disks/createorupdate#create_a_managed_disk_by_importing_an_unmanaged_blob_from_a_different_subscription.) a partir de un blob sin administrar.


## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de las distintas herramientas, técnicas y recomendaciones para migrar los recursos en las siguientes categorías de servicio:

- [Proceso](./germany-migration-compute.md)
- [Redes](./germany-migration-networking.md)
- [Web](./germany-migration-web.md)
- [Bases de datos](./germany-migration-databases.md)
- [Analytics](./germany-migration-analytics.md)
- [IoT](./germany-migration-iot.md)
- [Integración](./germany-migration-integration.md)
- [Identidad](./germany-migration-identity.md)
- [Seguridad](./germany-migration-security.md)
- [Herramientas de administración](./germany-migration-management-tools.md)
- [Elementos multimedia](./germany-migration-media.md)

