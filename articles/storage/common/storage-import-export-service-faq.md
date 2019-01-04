---
title: Preguntas frecuentes sobre el servicio Azure Import/Export | Microsoft Docs
description: Lea las respuestas a las preguntas frecuentes sobre el servicio Azure Import/Export.
author: alkohli
services: storage
ms.service: storage
ms.topic: article
ms.date: 12/13/2018
ms.author: alkohli
ms.component: common
ms.openlocfilehash: 8111d80d0888455fbdf3ccf37e723fe348a62bee
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2018
ms.locfileid: "53435011"
---
# <a name="azure-importexport-service-frequently-asked-questions"></a>Servicio Azure Import/Export: preguntas frecuentes 
Estas son algunas de las preguntas que puede plantearse, junto con sus respuestas, al usar el servicio Azure Import/Export para transferir datos a Azure Storage. Las preguntas y respuestas se organizan en las categorías siguientes:

- Acerca del servicio Import/Export
- Preparación de los discos para importación y exportación
- Trabajos de importación y exportación
- Envío de discos
- Varios 

## <a name="about-importexport-service"></a>Acerca del servicio Import/Export

### <a name="can-i-copy-azure-file-storage-using-the-azure-importexport-service"></a>¿Puedo copiar Azure File Storage mediante el servicio Azure Import/Export?

Sí. Sí, el servicio Azure Import/Export permite importar en Azure File Storage. Actualmente no admite la exportación de Azure Files.

### <a name="is-the-azure-importexport-service-available-for-csp-subscriptions"></a>¿Está disponible el servicio Azure Import/Export para suscripciones de CSP?

Sí. El servicio Azure Import/Export admite suscripciones a proveedores de soluciones en la nube (CSP).

### <a name="can-i-use-the-azure-importexport-service-to-copy-pst-mailboxes-and-sharepoint-data-to-o365"></a>¿Puedo usar el servicio Azure Import/Export para copiar buzones PST y datos de SharePoint en Office 365?

Sí. Para más información, vaya a [Importación de archivos PST o de datos de SharePoint en Office 365](https://technet.microsoft.com/library/ms.o365.cc.ingestionhelp.aspx).

### <a name="can-i-use-the-azure-importexport-service-to-copy-my-backups-offline-to-the-azure-backup-service"></a>¿Puedo usar el servicio Azure Import/Export para copiar las copias de seguridad sin conexión en el servicio Azure Backup?

Sí. Para más información, consulte [Flujo de trabajo de copia de seguridad sin conexión en Azure Backup](../../backup/backup-azure-backup-import-export.md).

### <a name="can-i-purchase-drives-for-importexport-jobs-from-microsoft"></a>¿Puedo adquirir unidades de Microsoft para los trabajos de importación y exportación?

 No. Deberá enviar sus propias unidades para los trabajos de importación y exportación.


## <a name="preparing-disks-for-importexport"></a>Preparación de discos para importación y exportación

### <a name="can-i-skip-the-drive-preparation-step-for-an-import-job-can-i-prepare-a-drive-without-copying"></a>¿Puedo omitir el paso de preparación de las unidades en un trabajo de importación? ¿Puedo preparar una unidad sin necesidad de copiar?

 No. Todas las unidades usadas para importar los datos se deben preparar con la herramienta WAImportExport de Azure. Use la herramienta para copiar también los datos en la unidad.

### <a name="do-i-need-to-perform-any-disk-preparation-when-creating-an-export-job"></a>¿Es necesario realizar alguna preparación en el disco al crear un trabajo de exportación?

 No. Se recomiendan algunas comprobaciones previas. Para comprobar el número de discos necesarios, use el comando PreviewExport de la herramienta WAImportExport. Para más información, consulte [Vista previa de uso de disco para un trabajo de exportación](https://msdn.microsoft.com/library/azure/dn722414.aspx). El comando le ayuda a obtener una vista previa del uso del disco para los blobs que ha seleccionado, en función del tamaño de las unidades que se van a usar. Compruebe también que puede leer y escribir en la unidad de disco duro que se va a enviar para el trabajo de exportación.

## <a name="importexport-jobs"></a>Trabajos de importación y exportación

### <a name="can-i-cancel-my-job"></a>¿Puedo cancelar un trabajo?
Sí. Puede cancelar un trabajo si el estado es **Creando** o **Enviando**. Superadas estas fases, no se puede cancelar el trabajo y este continúa hasta la fase final.

### <a name="how-long-can-i-view-the-status-of-completed-jobs-in-the-azure-portal"></a>¿Durante cuánto tiempo puedo visualizar el estado de los trabajos completados en Azure Portal?
Puede ver el estado de los trabajos completados durante 90 días. Los trabajos completados se eliminarán transcurridos 90 días.

### <a name="if-i-want-to-import-or-export-more-than-10-drives-what-should-i-do"></a>Si deseo importar o exportar más de 10 unidades, ¿qué debo hacer?
Un trabajo de importación o exportación solo puede hacer referencia a 10 unidades en un único trabajo. Para enviar más de 10 unidades de disco, debe crear varios trabajos. Las unidades asociadas al mismo trabajo se deben enviar juntas en el mismo paquete. Para más información e instrucciones cuando la capacidad de los datos abarca varios trabajos de importación de discos, póngase en contacto con Microsoft en bulkimport@microsoft.com. 

### <a name="the-uploaded-blob-shows-status-as-lease-expired-what-should-i-do"></a>El blob cargado muestra el estado como concesión expirada. ¿Cuál debo hacer?
Puede omitir el campo de concesión expirada. El servicio Import/Export toma la concesión del blob durante la carga para asegurarse de que ningún otro proceso puede actualizar el blob en paralelo. La concesión expirada implica que el servicio Import/Export ya no se está cargando en él y que el blob está disponible para su uso. 

## <a name="shipping-disks"></a>Envío de discos

### <a name="what-is-the-maximum-number-of-hdd-for-in-one-shipment"></a>¿Cuántas unidades de disco duro se pueden incluir como máximo en un envío?
No hay ningún límite para el número de unidades de disco duro en un solo envío. Si los discos pertenecen a varios trabajos, se recomienda lo siguiente: 
- Etiquetar los discos con los nombres de los trabajos correspondientes
- Actualizar los trabajos con un número de seguimiento que lleve el sufijo -1, -2, etc.

### <a name="should-i-include-anything-other-than-the-hdd-in-my-package"></a>¿Debo incluir alguna otra cosa, aparte de la unidad de disco duro, en el paquete?
Envíe solo las unidades de disco duro en el paquete de envío. No incluya elementos como cables de alimentación o cables USB.

### <a name="do-i-have-to-ship-my-drives-using-fedex-or-dhl"></a>¿Es necesario enviar mis unidades con FedEx o DHL?
Puede enviar las unidades al centro de datos de Azure con cualquier empresa de transporte conocida, como FedEx, DHL o UPS, o bien mediante el servicio postal de los Estados Unidos. Sin embargo, para el envío de devolución de las unidades desde el centro de datos, debe proporcionar:

- Un número de cuenta de FedEx en Estados Unidos y Europa, o
- Un número de cuenta de DHL en las regiones de Asia y Australia.

### <a name="are-there-any-restrictions-with-shipping-my-drive-internationally"></a>¿Existe alguna restricción internacional en el envío de las unidades?
Tenga en cuenta que es posible que los medios físicos que está enviando deban cruzar alguna frontera internacional. Usted es el responsable de asegurar que los medios y datos físicos se importan o exportan de acuerdo con todas las normativas aplicables. Antes de enviar los medios físicos, pida asesoramiento para comprobar que los medios y datos se pueden enviar legalmente al centro de datos identificado. De este modo, se asegurará de que llegan a Microsoft de manera puntual.

### <a name="when-creating-a-job-the-shipping-address-is-a-location-that-is-different-from-my-storage-account-location-what-should-i-do"></a>Al crear un trabajo, la dirección de envío es una ubicación distinta de la ubicación de la cuenta de almacenamiento. ¿Cuál debo hacer?

Algunas ubicaciones de la cuenta de almacenamiento se asignan a ubicaciones de envío alternativas. Ubicaciones de envío anteriormente disponibles se pueden asignar también temporalmente a ubicaciones alternativas. Compruebe siempre la dirección de envío proporcionada durante la creación del trabajo antes de enviar las unidades.

### <a name="when-shipping-my-drive-the-carrier-asks-for-the-data-center-contact-address-and-phone-number-what-should-i-provide"></a>Al enviar la unidad, la empresa de transporte le pedirá la dirección de contacto y el número de teléfono del centro de datos. ¿Qué debo proporcionar?

El número de teléfono y la dirección del centro de datos se proporcionan como parte de la creación del trabajo.


## <a name="miscellaneous"></a>Varios

### <a name="what-happens-if-i-accidentally-send-an-hdd-that-does-not-conform-to-the-supported-requirements"></a>¿Qué sucede si envío por error una unidad de disco duro que no cumple con los requisitos de compatibilidad?

El centro de datos de Azure le devolverá la unidad que no cumple con los requisitos de compatibilidad. Si solo algunos de los dispositivos del paquete cumplen con los requisitos de compatibilidad, esas unidades se procesarán, y las unidades que no cumplan los requisitos se le devolverán.

### <a name="does-the-service-format-the-drives-before-returning-them"></a>¿El servicio formatea las unidades antes de devolverlas?

 No. Todas las unidades están cifradas con BitLocker.

### <a name="how-can-i-access-data-that-is-imported-by-this-service"></a>¿Cómo puedo acceder a los datos importados por este servicio?

Use Azure Portal o el [Explorador de Storage](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) para acceder a los datos de la cuenta de almacenamiento de Azure.  

### <a name="after-the-import-is-complete-what-does-my-data-look-like-in-the-storage-account-is-my-directory-hierarchy-preserved"></a>Cuando finalice el trabajo de importación, ¿cómo se muestran los datos en la cuenta de almacenamiento? ¿Se conserva la jerarquía de directorios?

Al preparar una unidad de disco duro para un trabajo de importación, el destino se especifica mediante el campo DstBlobPathOrPrefix del archivo CSV de conjunto de datos. Este es el contenedor de destino de la cuenta de almacenamiento donde se copian los datos de la unidad de disco duro. Dentro de él se crean directorios virtuales para las carpetas de la unidad de disco duro y blobs para los archivos. 

### <a name="if-a-drive-has-files-that-already-exist-in-my-storage-account-does-the-service-overwrite-existing-blobs-or-files"></a>Si una unidad tiene archivos que ya existen en la cuenta de almacenamiento, ¿el servicio sobrescribirá los blobs o archivos existentes?

Depende. Al preparar la unidad, puede especificar que los archivos de destino se sobrescriban u omitan mediante el campo del archivo CSV de conjunto de datos llamado Disposition:<rename|no-overwrite|overwrite>. De forma predeterminada, el servicio cambia el nombre de los nuevos archivos en lugar de sobrescribir los blobs o archivos existentes.

### <a name="is-the-waimportexport-tool-compatible-with-32-bit-operating-systems"></a>¿Es compatible la herramienta WAImportExport con sistemas operativos de 32 bits?
 No. La herramienta WAImportExport solo es compatible con sistemas operativos de Windows de 64 bits. Para obtener una lista completa de sistemas operativos admitidos, vaya a [Sistemas operativos compatibles](https://docs.microsoft.com/azure/storage/common/storage-import-export-requirements). 


### <a name="what-is-the-maximum-block-blob-and-page-blob-size-supported-by-azure-importexport"></a>¿Qué tamaño máximo de blobs en bloques y blobs en páginas admite Azure Import/Export?

El tamaño máximo aproximado de blobs en bloques admitido es de 4,768 TB o 5.000.000 MB.
El tamaño máximo de blobs en páginas es de 1 TB.


### <a name="does-azure-importexport-support-aes-256-encryption"></a>¿Admite el servicio Azure Import/Export el cifrado AES 256?
El servicio Azure Import/Export usa el cifrado de Bitlocker AES-128 de forma predeterminada. Puede cambiarlo a AES-256 mediante el cifrado manual con Bitlocker antes de que se copien los datos. 

- Si usa [WAImportExport V1](https://download.microsoft.com/download/0/C/D/0CD6ABA7-024F-4202-91A0-CE2656DCE413/WaImportExportV1.zip), a continuación se muestra un comando de ejemplo
    ```
    WAImportExport PrepImport /sk:<StorageAccountKey> /csas:<ContainerSas> /t: <TargetDriveLetter> [/format] [/silentmode] [/encrypt] [/bk:<BitLockerKey>] [/logdir:<LogDirectory>] /j:<JournalFile> /id:<SessionId> /srcdir:<SourceDirectory> /dstdir:<DestinationBlobVirtualDirectory> [/Disposition:<Disposition>] [/BlobType:<BlockBlob|PageBlob>] [/PropertyFile:<PropertyFile>] [/MetadataFile:<MetadataFile>] 
    ```
- Si usa [WAImportExport V2](https://www.microsoft.com/en-us/download/details.aspx?id=55280), especifique "AlreadyEncrypted" y proporcione la clave en el archivo driveset.csv.
    ```
    DriveLetter,FormatOption,SilentOrPromptOnFormat,Encryption,ExistingBitLockerKey
    G,AlreadyFormatted,SilentMode,AlreadyEncrypted,060456-014509-132033-080300-252615-584177-672089-411631 |
    ```

## <a name="next-steps"></a>Pasos siguientes

* [¿Qué es Azure Import/Export?](storage-import-export-service.md)


