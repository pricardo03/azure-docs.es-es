---
title: Problemas conocidos con Azure Data Lake Storage Gen2 | Microsoft Docs
description: Obtenga información sobre las limitaciones y problemas conocidos con Azure Data Lake Storage Gen2
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 07/18/2019
ms.author: normesta
ms.openlocfilehash: 75e0aa0847d44df40a4823d98460b011addab4d7
ms.sourcegitcommit: 770b060438122f090ab90d81e3ff2f023455213b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2019
ms.locfileid: "68305747"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Problemas conocidos con Azure Data Lake Storage Gen2

En este artículo se enumeran las características y herramientas que aún no son compatibles o que son parcialmente compatibles con las cuentas de almacenamiento que tienen un espacio de nombres jerárquico (Azure Data Lake Storage Gen2).

<a id="blob-apis-disabled" />

## <a name="blob-storage-apis"></a>API de Blob Storage

Las API de Blob Storage están deshabilitadas para evitar problemas de operatividad en las funciones que pueden surgir debido a que las API de Blob Storage no pueden interoperarse con las API de Azure Data Lake Gen2.

> [!NOTE]
> Si se inscribe en la versión preliminar pública del acceso multiprotocolo en Data Lake Storage, las API de blob y las API de Data Lake Storage Gen2 pueden operar en los mismos datos. Para más información, consulte [Acceso multiprotocolo en Data Lake Storage](data-lake-storage-multi-protocol-access.md).

### <a name="what-to-do-with-existing-tools-applications-and-services"></a>Qué hacer con las herramientas, aplicaciones y servicios existentes

Si alguno de los elementos anteriores usa las API de blob y desea usarlas para trabajar con todo el contenido que cargó en su cuenta, tiene dos opciones.

* **Opción 1**: No habilite un espacio de nombres jerárquico en su cuenta de almacenamiento de blobs hasta que las API de blob puedan interoperar con las API de Azure Data Lake Gen2. Cuando usa una cuenta de almacenamiento sin un espacio de nombres jerárquico, carece de acceso a características específicas de Data Lake Storage Gen2, como listas de control de acceso al directorio y al sistema de archivos.

* **Opción 2**: Inscríbase en la versión preliminar pública del [acceso multiprotocolo en Data Lake Storage](data-lake-storage-multi-protocol-access.md). Las herramientas y las aplicaciones que llaman a las API de blob, así como las características de almacenamiento de blobs, como los registros de diagnóstico, pueden trabajar con cuentas que tienen un espacio de nombres jerárquico.

### <a name="what-to-do-if-you-used-blob-apis-to-load-data-before-blob-apis-were-disabled"></a>Qué hacer si se usaron las API de Blob para cargar los datos antes de deshabilitar las API de Blob

Si usó estas API para cargar datos antes de haberlas deshabilitado y tiene un requisito de producción para tener acceso a esos datos, póngase en contacto con Soporte técnico de Microsoft con la siguiente información:

> [!div class="checklist"]
> * Id. de suscripción (el GUID, no el nombre).
> * Nombres de la cuenta de almacenamiento.
> * Si le afecta activamente en producción y, si es así, ¿para qué cuentas de almacenamiento?
> * Aunque no le afecte activamente en producción, indíquenos si necesita que estos datos se copien en otra cuenta de almacenamiento por alguna razón y, si es así, cuál es el motivo.

En estas circunstancias, podemos restaurar el acceso a la API de Blob durante un período limitado para que pueda copiar estos datos en una cuenta de almacenamiento que no tenga habilitada la característica de espacios de nombres jerárquicos.

### <a name="issues-and-limitations-with-using-blob-apis-on-accounts-that-have-a-hierarchical-namespace"></a>Problemas y limitaciones con el uso de API de blob en cuentas que tienen un espacio de nombres jerárquico

Si se inscribe en la versión preliminar pública del acceso multiprotocolo en Data Lake Storage, las API de blob y las API de Data Lake Storage Gen2 pueden operar en los mismos datos.

En esta sección se describen los problemas y las limitaciones del uso de las API de blob y de las API de Data Lake Storage Gen2 para operar en los mismos datos.

Las siguientes API REST de blob no se admiten:

* [Put Blob (Page)](https://docs.microsoft.com/rest/api/storageservices/put-blob) [Poner blob (página)]
* [Put Page](https://docs.microsoft.com/rest/api/storageservices/put-page)
* [Get Page Ranges](https://docs.microsoft.com/rest/api/storageservices/get-page-ranges) (Conseguir intervalos de páginas)
* [Incremental Copy Blob](https://docs.microsoft.com/rest/api/storageservices/incremental-copy-blob) (Copia incremental del blob)
* [Put Page from URL](https://docs.microsoft.com/rest/api/storageservices/put-page-from-url) (Poner página de dirección URL)
* [Put Blob (Append)](https://docs.microsoft.com/rest/api/storageservices/put-blob) [Poner blob (anexar)]
* [Append Block](https://docs.microsoft.com/rest/api/storageservices/append-block)
* [Append Block from URL](https://docs.microsoft.com/rest/api/storageservices/append-block-from-url) (Anexar bloque desde dirección URL)

* No puede usar las API de blob y las API de Data Lake Storage para escribir en la misma instancia de un archivo.

* Si escribe en un archivo con la API de Data Lake Storage Gen2, los bloques de ese archivo no estarán visibles para las llamadas a la API de blob [Get Block List](https://docs.microsoft.comrest/api/storageservices/get-block-list).

* Puede sobrescribir un archivo con las API de Data Lake Storage Gen2 o con las API de blob. No afectará a las propiedades del archivo.

* Cuando use la operación [List Blobs](https://docs.microsoft.com/rest/api/storageservices/list-blobs) (Enumerar blobs) sin especificar un delimitador, los resultados incluirán tanto los directorios como los blobs.

  Si decide usar un delimitador, use solo una barra diagonal (`/`). Este es el único delimitador admitido.

* Si usa la API [Delete Blob](https://docs.microsoft.com/rest/api/storageservices/delete-blob) para eliminar un directorio, solo se eliminará si está vacío.

  Esto significa que no puede utilizar la API de blob para eliminar los directorios de forma recursiva.

## <a name="issues-with-unmanaged-virtual-machine-vm-disks"></a>Problemas con los discos no administrados de máquina virtual (VM)

No se admiten discos de máquina virtual no administrados en cuentas que tengan un espacio de nombres jerárquico. Si desea habilitar un espacio de nombres jerárquico en una cuenta de almacenamiento, coloque los discos de máquina virtual no administrados en una cuenta de almacenamiento que no tenga habilitada la característica de espacio de nombres jerárquico.


## <a name="support-for-other-blob-storage-features"></a>Compatibilidad con otras características de almacenamiento de blobs

En la siguiente tabla se enumeran las demás características y herramientas que aún no son compatibles o que son parcialmente compatibles con las cuentas de almacenamiento que tienen un espacio de nombres jerárquico (Azure Data Lake Storage Gen2).

| Característica o herramienta    | Más información    |
|--------|-----------|
| **Las API para las cuentas de almacenamiento de Data Lake Storage Gen2** | Compatibilidad parcial <br><br>El acceso multiprotocolo en Data Lake Storage está actualmente en la versión preliminar pública. Esta versión preliminar le permite usar las API de blob en los SDK para .NET, Java y Python con cuentas que tengan un espacio de nombres jerárquico.  Los SDK todavía no contienen API que le permitan interactuar con directorios o establecer listas de control de acceso (ACL). Para realizar esas funciones, puede usar las API **REST** de Data Lake Storage Gen2. |
| **AzCopy** | Compatibilidad con versiones específicas <br><br>Use solo la versión más reciente de AzCopy ([AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)). No se admiten versiones anteriores de AzCopy, como AzCopy v8.1.|
| **Directivas de administración del ciclo de vida de Azure Blob Storage** | Solo se admiten si se inscribe en la versión preliminar pública del [acceso multiprotocolo en Data Lake Storage](data-lake-storage-multi-protocol-access.md). Los niveles de acceso esporádico y de almacenamiento de archivo solo se admiten en la versión preliminar. Todavía no se admite la eliminación de instantáneas de blob. |
| **Azure Content Delivery Network** | Todavía no se admite|
| **Azure Search** |Todavía no se admite|
| **Explorador de Azure Storage** | Compatibilidad con versiones específicas <br><br>Use solo la versión `1.6.0` o superior. <br>La versión `1.6.0` está disponible como [descarga gratuita](https://azure.microsoft.com/features/storage-explorer/).|
| **Listas de control de acceso del contenedor de blobs** |Todavía no se admite|
| **Blobfuse** |Todavía no se admite|
| **Dominios personalizados** |Todavía no se admite|
| **Explorador del sistema de archivos** | Compatibilidad limitada |
| **Almacenamiento inmutable** |Todavía no se admite <br><br>El almacenamiento inmutable ofrece la capacidad de almacenar datos en estado [WORM](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutable-storage).|
| **Niveles de objeto** |Los niveles de acceso esporádico y de almacenamiento de archivo solo se admiten si se inscribe en la versión preliminar del [acceso multiprotocolo en Data Lake Storage](data-lake-storage-multi-protocol-access.md). <br><br> Todavía no se admiten los demás niveles de acceso.|
| **Compatibilidad con PowerShell y CLI** | Funcionalidad limitada <br><br>Se admiten las operaciones de administración como la creación de una cuenta. Las operaciones del plano de datos, como la carga y descarga de archivos, están en la versión preliminar pública [como parte del acceso multiprotocolo de Data Lake Storage](data-lake-storage-multi-protocol-access.md). Todavía no se admite el uso de directorios y la configuración de listas de control de acceso (ACL). |
| **Sitios web estáticos** |Todavía no se admite <br><br>En concreto, la capacidad de incluir los archivos en [los sitios web estáticos](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website).|
| **Aplicaciones de terceros** | Compatibilidad limitada <br><br>Las aplicaciones de terceros que usan las API REST para funcionar seguirán funcionando si se usan con Data Lake Storage Gen2. <br>Las aplicaciones que llaman a las API de blob probablemente funcionarán si se inscribe en la versión preliminar pública del [acceso multiprotocolo en Data Lake Storage](data-lake-storage-multi-protocol-access.md). 
| **Características de control de versiones** |Todavía no se admite <br><br>Esto incluye [instantáneas](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob) y [eliminación temporal](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete).|


