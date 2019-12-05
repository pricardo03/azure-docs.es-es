---
title: Problemas conocidos con Azure Data Lake Storage Gen2 | Microsoft Docs
description: Obtenga información sobre las limitaciones y problemas conocidos con Azure Data Lake Storage Gen2
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 11/03/2019
ms.author: normesta
ms.reviewer: jamesbak
ms.openlocfilehash: 3ea77eb5dd8a03f877164179e3accc3a6f6d0aef
ms.sourcegitcommit: a678f00c020f50efa9178392cd0f1ac34a86b767
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2019
ms.locfileid: "74548329"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Problemas conocidos con Azure Data Lake Storage Gen2

En este artículo se enumeran las características y herramientas que aún no son compatibles o que son parcialmente compatibles con las cuentas de almacenamiento que tienen un espacio de nombres jerárquico (Azure Data Lake Storage Gen2).

<a id="blob-apis-disabled" />

## <a name="issues-and-limitations-with-using-blob-apis"></a>Problemas y limitaciones con el uso de API de Blob

Las API de Blob y Data Lake Storage Gen2 API pueden operar en los mismos datos.

En esta sección se describen los problemas y las limitaciones del uso de las API de blob y de las API de Data Lake Storage Gen2 para operar en los mismos datos.

* No puede usar las API de blob y las API de Data Lake Storage para escribir en la misma instancia de un archivo. Si escribe en un archivo con la API de Data Lake Storage Gen2, los bloques de ese archivo no estarán visibles para las llamadas a la API de blob [Get Block List](https://docs.microsoft.com/rest/api/storageservices/get-block-list). Puede sobrescribir un archivo con las API de Data Lake Storage Gen2 o con las API de blob. No afectará a las propiedades del archivo.

* Cuando use la operación [List Blobs](https://docs.microsoft.com/rest/api/storageservices/list-blobs) (Enumerar blobs) sin especificar un delimitador, los resultados incluirán tanto los directorios como los blobs. Si decide usar un delimitador, use solo una barra diagonal (`/`). Este es el único delimitador admitido.

* Si usa la API [Delete Blob](https://docs.microsoft.com/rest/api/storageservices/delete-blob) para eliminar un directorio, solo se eliminará si está vacío. Esto significa que no puede utilizar la API de blob para eliminar los directorios de forma recursiva.

Las siguientes API REST de blob no se admiten:

* [Put Blob (Page)](https://docs.microsoft.com/rest/api/storageservices/put-blob) [Poner blob (página)]
* [Put Page](https://docs.microsoft.com/rest/api/storageservices/put-page)
* [Get Page Ranges](https://docs.microsoft.com/rest/api/storageservices/get-page-ranges) (Conseguir intervalos de páginas)
* [Incremental Copy Blob](https://docs.microsoft.com/rest/api/storageservices/incremental-copy-blob) (Copia incremental del blob)
* [Put Page from URL](https://docs.microsoft.com/rest/api/storageservices/put-page-from-url) (Poner página de dirección URL)
* [Put Blob (Append)](https://docs.microsoft.com/rest/api/storageservices/put-blob) [Poner blob (anexar)]
* [Append Block](https://docs.microsoft.com/rest/api/storageservices/append-block)
* [Append Block from URL](https://docs.microsoft.com/rest/api/storageservices/append-block-from-url) (Anexar bloque desde dirección URL)

No se admiten discos de máquina virtual no administrados en cuentas que tengan un espacio de nombres jerárquico. Si desea habilitar un espacio de nombres jerárquico en una cuenta de almacenamiento, coloque los discos de máquina virtual no administrados en una cuenta de almacenamiento que no tenga habilitada la característica de espacio de nombres jerárquico.

<a id="api-scope-data-lake-client-library" />

## <a name="api-scope-for-data-lake-storage-client-library-for-sdk-powershell-and-cli"></a>Ámbito de API de la biblioteca cliente de Data Lake Storage para SDK, PowerShell y CLI

### <a name="filesystem-support-in-sdks"></a>Compatibilidad del sistema de archivos en SDK

- .NET, Java y Python se encuentran en versión preliminar pública. No se admiten otros SDK actualmente.
- Las operaciones de ACL Get y Set no son recursivas actualmente.

### <a name="filesystem-support-in-powershell-and-azure-cli"></a>Compatibilidad del sistema de archivos en PowerShell y la CLI de Azure

Las operaciones de ACL Get y Set no son recursivas actualmente.

## <a name="support-for-other-blob-storage-features"></a>Compatibilidad con otras características de Blob Storage

En la siguiente tabla se enumeran las demás características y herramientas que aún no son compatibles o que son parcialmente compatibles con las cuentas de almacenamiento que tienen un espacio de nombres jerárquico (Azure Data Lake Storage Gen2).

| Característica o herramienta    | Más información    |
|--------|-----------|
| **AzCopy** | Compatibilidad con versiones específicas <br><br>Use solo la versión más reciente de AzCopy ([AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)). No se admiten versiones anteriores de AzCopy, como AzCopy v8.1.|
| **Directivas de administración del ciclo de vida de Azure Blob Storage** | Se admiten las directivas de administración del ciclo de vida (versión preliminar).  Se admiten todos los niveles de acceso. El nivel de acceso de archivo se encuentra actualmente en versión preliminar. Todavía no se admite la eliminación de instantáneas de blob. <br><br> Actualmente, hay algunos errores que afectan a las directivas de administración del ciclo de vida y al nivel de acceso de archivo.  Regístrese [aquí](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2EUNXd_ZNJCq_eDwZGaF5VURjFLTDRGS0Q4VVZCRFY5MUVaTVJDTkROMi4u) para obtener la versión preliminar de las directivas de administración del ciclo de vida y el nivel de acceso de archivo.   |
| **Azure Content Delivery Network** | Todavía no se admite|
| **Azure Search** |Compatible (versión preliminar)|
| **Explorador de Azure Storage** | Compatibilidad con versiones específicas. <br><br>Use solo las versiones `1.6.0` o superiores. <br> Actualmente hay un error de almacenamiento que afecta a la versión `1.11.0` que puede dar lugar a errores de autenticación en ciertos escenarios. Se está implementando una corrección para el error de almacenamiento, pero, como solución alternativa, se recomienda usar la versión `1.10.x`, que está disponible como [descarga gratuita](https://docs.microsoft.com/azure/vs-azure-tools-storage-explorer-relnotes). El error de almacenamiento no afecta a la versión `1.10.x`.|
| **Listas de control de acceso del contenedor de blobs** |Todavía no se admite|
| **Blobfuse** |Todavía no se admite|
| **Dominios personalizados** |Todavía no se admite|
| **Explorador de Storage en Azure Portal** | Compatibilidad limitada. Las ACL aún no se admiten. |
| **Registro de diagnóstico** |Se admiten los registros de diagnóstico (versión preliminar).<br><br>Actualmente no se admite la habilitación de registros en Azure Portal. Este es un ejemplo de cómo habilitar los registros mediante PowerShell. <br><br>`$storageAccount = Get-AzStorageAccount -ResourceGroupName <resourceGroup> -Name <storageAccountName>`<br><br>`Set-AzStorageServiceLoggingProperty -Context $storageAccount.Context -ServiceType Blob -LoggingOperations read,write,delete -RetentionDays <days>`. <br><br>Asegúrese de especificar `Blob` como valor del parámetro `-ServiceType` como se muestra en este ejemplo. <br><br>Actualmente, Explorador de Azure Storage no se puede usar para ver registros de diagnóstico. Para ver los registros, use AzCopy o SDK.
| **Almacenamiento inmutable** |Todavía no se admite <br><br>El almacenamiento inmutable ofrece la capacidad de almacenar datos en estado [WORM](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutable-storage).|
| **Niveles de objeto** |Se admiten las capas de almacenamiento en frío y en archivos. El nivel de archivo está en versión preliminar. Todavía no se admiten los demás niveles de acceso. <br><br> Actualmente, hay algunos errores que afectan al nivel de acceso de archivo.  Regístrese [aquí](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR2EUNXd_ZNJCq_eDwZGaF5VURjFLTDRGS0Q4VVZCRFY5MUVaTVJDTkROMi4u) para obtener la versión preliminar del nivel de acceso de archivo.|
| **Compatibilidad con PowerShell y CLI** | Funcionalidad limitada <br><br>Se admiten las operaciones de blob. Todavía no se admite el uso de directorios y la configuración de listas de control de acceso (ACL). |
| **Sitios web estáticos** |Todavía no se admite <br><br>En concreto, la capacidad de incluir los archivos en [los sitios web estáticos](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website).|
| **Aplicaciones de terceros** | Compatibilidad limitada <br><br>Las aplicaciones de terceros que usan las API REST para funcionar seguirán funcionando si se usan con Data Lake Storage Gen2. <br>Lo más probable es que las aplicaciones que llaman a las API de Blob funcionen.|
|**eliminación temporal** |Todavía no se admite|
| **Características de control de versiones** |Todavía no se admite <br><br>Esto incluye la [eliminación temporal](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) y otras características de control de versiones como [instantáneas](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob).|


