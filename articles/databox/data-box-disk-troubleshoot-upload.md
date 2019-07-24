---
title: Solución de problemas de carga de datos en Azure Data Box Disk con registros | Microsoft Docs
description: Se describe cómo utilizar los registros y solucionar problemas al cargar datos en Azure Data Box Disk.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 06/14/2019
ms.author: alkohli
ms.openlocfilehash: 6f3ac38c3eac968bd2f7ec2aada435466d3ff279
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/14/2019
ms.locfileid: "67148124"
---
# <a name="understand-logs-to-troubleshoot-data-upload-issues-in-azure-data-box-disk"></a>Descripción de registros para solucionar incidencias de carga de datos en Azure Data Box Disk

Este artículo se aplica a Microsoft Azure Data Box Disk y describe los problemas que se producen al cargar datos en Azure.

## <a name="data-upload-logs"></a>Registros de carga de datos

Cuando los datos se cargan en Azure en el centro de datos, se generan los archivos `_error.xml` y `_verbose.xml`. Estos registros se cargan en la misma cuenta de almacenamiento que se usó para cargar los datos. A continuación se muestra un ejemplo de `_error.xml`.
    
```xml
 <?xml version="1.0" encoding="utf-8"?>
    <DriveLog Version="2018-10-01">
      <DriveId>18041C582D7E</DriveId>
      <Summary>
     <!--Summary for validation and data copy to Azure -->
        <ValidationErrors>
          <None Count="8" />
        </ValidationErrors>
        <CopyErrors>
          <Completed Count="5" Description="No errors encountered" />
          <ContainerRenamed Count="3" Description="Renamed the container as the original container name does not follow Azure conventions." />
        </CopyErrors>
      </Summary>
    <!--List of renamed containers with the new names, new file path in Azure, original invalid file path, and size -->
      <Blob Status="ContainerRenamed">
        <BlobPath>databox-c2073fd1cc379d83e03d6b7acce23a6cf29d1eef/private.vhd</BlobPath>
        <OriginalFilePath>\PageBlob\pageblob test\private.vhd</OriginalFilePath>
        <SizeInBytes>10490880</SizeInBytes>
      </Blob>
      <Blob Status="ContainerRenamed">
        <BlobPath>databox-c2073fd1cc379d83e03d6b7acce23a6cf29d1eef/resource.vhd</BlobPath>
        <OriginalFilePath>\PageBlob\pageblob test\resource.vhd</OriginalFilePath>
        <SizeInBytes>71528448</SizeInBytes>
      </Blob>
      <Blob Status="ContainerRenamed">
        <BlobPath>databox-c2073fd1cc379d83e03d6b7acce23a6cf29d1eef/role.vhd</BlobPath>
        <OriginalFilePath>\PageBlob\pageblob test\role.vhd</OriginalFilePath>
        <SizeInBytes>10490880</SizeInBytes>
      </Blob>
      <Status>CompletedWithErrors</Status>
    </DriveLog>
```

## <a name="download-logs"></a>Descargar registros

Hay dos maneras de buscar y descargar los registros de diagnóstico.

- Si se producen errores durante la carga de los datos en Azure, en el portal se muestra la ruta de acceso a la carpeta donde se encuentran los registros de diagnóstico.

    ![Vínculos a registros en el portal](./media/data-box-disk-troubleshoot-upload/data-box-disk-portal-logs.png)

- Vaya a la cuenta de almacenamiento asociada al pedido de Data Box. Vaya a **Blob service > Examinar blobs** y busque el blob correspondiente a la cuenta de almacenamiento. Vaya a **waies**.

    ![Registros de copia 2](./media/data-box-disk-troubleshoot/data-box-disk-copy-logs2.png)

En cada caso, verá los registros de errores y los registros detallados. Seleccione cada registro y descargue una copia local.


## <a name="data-upload-errors"></a>Errores de carga de datos

En la tabla siguiente se resumen los errores generados al cargar los datos en Azure.

| Código de error | DESCRIPCIÓN                        |
|-------------|------------------------------|
|`None` |  Se completó correctamente.           |
|`Renamed` | El nombre del blob se cambió correctamente.  |                                                            |
|`CompletedWithErrors` | La carga se completó con errores. Los detalles de los archivos con errores se incluyen en el archivo de registro.  |
|`Corrupted`|El valor de CRC calculado durante la ingesta de datos no coincide con el valor de CRC calculado durante la carga.  |  
|`StorageRequestFailed` | La solicitud de Azure Storage no se realizó.   |     |
|`LeasePresent` | Esté artículo se ha concedido y lo usa otro usuario. |
|`StorageRequestForbidden` |No se pudo cargar debido a problemas de autenticación. |
|`ManagedDiskCreationTerminalFailure` | No se pudo cargar como discos administrados. Los archivos están disponibles en la cuenta de almacenamiento provisional como blobs en páginas. Puede convertir manualmente los blobs en páginas a discos administrados.  |
|`DiskConversionNotStartedTierInfoMissing` | Puesto que el archivo de disco duro virtual se copió fuera de las carpetas del nivel creadas previamente, no se ha creado un disco administrado. El archivo se carga como blob en páginas en la cuenta de almacenamiento provisional que se especificó durante la creación del pedido. Puede convertirlo manualmente en un disco administrado.|
|`InvalidWorkitem` | No se pudieron cargar los datos ya que no se ajustan a las convenciones de nomenclatura y límites de Azure.|
|`InvalidPageBlobUploadAsBlockBlob` | Se cargaron como blobs en bloques en un contenedor con el prefijo `databoxdisk-invalid-pb-`.|
|`InvalidAzureFileUploadAsBlockBlob` | Se cargaron como blobs en bloques en un contenedor con el prefijo `databoxdisk-invalid-af`-.|
|`InvalidManagedDiskUploadAsBlockBlob` | Se cargaron como blobs en bloques en un contenedor con el prefijo `databoxdisk-invalid-md`-.|
|`InvalidManagedDiskUploadAsPageBlob` |Se cargaron como blobs en páginas en un contenedor con el prefijo `databoxdisk-invalid-md-`. |
|`MovedToOverflowShare` |Los archivos se cargaron en un nuevo recurso compartido porque el tamaño del recurso compartido original superaba el límite de tamaño máximo de Azure. El nombre del nuevo recurso compartido es el nombre original con el sufijo `-2`.   |
|`MovedToDefaultAzureShare` |Los archivos que no formaban parte de ninguna carpeta se cargaron en un recurso compartido predeterminado. El nombre del recurso compartido comienza por `databox-`. |
|`ContainerRenamed` |El contenedor de estos archivos no se ajustaba a las convenciones de nomenclatura de Azure y se le cambió el nombre. El nuevo nombre empieza por `databox-` e incluye el hash SHA1 del nombre original como sufijo. |
|`ShareRenamed` |El recurso compartido de estos archivos no se ajustaba a las convenciones de nomenclatura de Azure y se le cambió el nombre. El nuevo nombre empieza por `databox-` e incluye el hash SHA1 del nombre original como sufijo. |
|`BlobRenamed` |Estos archivos no se ajustaban a las convenciones de nomenclatura de Azure y se les cambió el nombre. Compruebe el nuevo nombre en el campo `BlobPath`. |
|`FileRenamed` |Estos archivos no se ajustaban a las convenciones de nomenclatura de Azure y se les cambió el nombre. Compruebe el nuevo nombre en el campo `FileStoragePath`. |
|`DiskRenamed` |Estos archivos no se ajustaban a las convenciones de nomenclatura de Azure y se les cambió el nombre. Compruebe el nuevo nombre en el campo `BlobPath`. |


## <a name="next-steps"></a>Pasos siguientes

- [Apertura de una incidencia de soporte técnico en Data Box Disk](data-box-disk-contact-microsoft-support.md).
