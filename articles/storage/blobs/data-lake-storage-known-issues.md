---
title: Problemas conocidos con Azure Data Lake Storage Gen2 | Microsoft Docs
description: Obtenga información sobre las limitaciones y problemas conocidos con Azure Data Lake Storage Gen2
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: normesta
ms.openlocfilehash: 27adc0eeeabed2b1f2e86f301a60604a3d358b82
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65464717"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Problemas conocidos con Azure Data Lake Storage Gen2

En este artículo se enumera las características y herramientas que aún no son compatibles o parcialmente compatibles con las cuentas de almacenamiento que tienen un espacio de nombres jerárquico (Azure Data Lake Storage Gen2).

<a id="blob-apis-disabled" />

## <a name="blob-storage-apis"></a>API de Blob Storage

API de BLOB storage están deshabilitados para evitar problemas de acceso accidental de datos que podrían surgir porque todavía no puede interoperar con API de Azure Data Lake Gen2 API de Blob Storage.

### <a name="what-to-do-with-existing-tools-applications-and-services"></a>Qué hacer con los servicios, aplicaciones y herramientas existentes

Si desea que cualquiera de estas API de Blob de uso y se usan para trabajar con todo el contenido que se carga en su cuenta, a continuación, no habilite un espacio de nombres jerárquico en su cuenta de almacenamiento de blobs hasta que la API de Blob interaccione con API de Azure Data Lake Gen2.

Con una cuenta de almacenamiento sin un espacio de nombres jerárquico significa que, a continuación, no tiene acceso a características específicas de Data Lake Storage Gen2, como archivos y directorios listas de control de acceso del sistema.

### <a name="what-to-do-with-unmanaged-virtual-machine-vm-disks"></a>Qué hacer con discos no administrados de máquina Virtual (VM)

Éstos dependen de la API de Blob Storage deshabilitado, por lo que si desea habilitar un espacio de nombres jerárquico en una cuenta de almacenamiento, puede colocarlos en una cuenta de almacenamiento que no tiene habilitada la característica de espacio de nombres jerárquico.

### <a name="what-to-do-if-you-used-blob-apis-to-load-data-before-blob-apis-were-disabled"></a>Qué hacer si usa las API de Blob para cargar los datos antes de que se han deshabilitado las API de Blob

Si usó estas API para cargar datos antes de haberlas deshabilitado y tiene un requisito de producción para tener acceso a esos datos, póngase en contacto con Soporte técnico de Microsoft con la siguiente información:

> [!div class="checklist"]
> * Id. de suscripción (el GUID, no el nombre).
> * Nombres de la cuenta de almacenamiento.
> * ¿Si le afecta activamente en producción y, si es así, qué cuentas de almacenamiento?
> * Aunque no le afecte activamente en producción, indíquenos si necesita que estos datos se copien en otra cuenta de almacenamiento por alguna razón y, si es así, cuál es el motivo.

En estas circunstancias, podemos restaurar el acceso a la API de Blob durante un período limitado para que pueda copiar estos datos en una cuenta de almacenamiento que no tenga habilitada la característica de espacios de nombres jerárquicos.

## <a name="all-other-features-and-tools"></a>Todas las otras características y herramientas

En la tabla siguiente se enumera todas las otras características y herramientas que aún no son compatibles o parcialmente compatibles con las cuentas de almacenamiento que tienen un espacio de nombres jerárquico (Azure Data Lake Storage Gen2).

| Característica o herramienta    | Más información    |
|--------|-----------|
| **API para las cuentas de almacenamiento de Data Lake Storage Gen2** | Compatibilidad parcial <br><br>Puede usar Data Lake Storage Gen2 **REST** API, pero las API en otros SDK de Blob, como los SDK. NET, Java, Python no están aún disponibles.|
| **AzCopy** | Compatibilidad con versiones específicas <br><br>Use solo la versión más reciente de AzCopy ([AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)). No se admiten versiones anteriores de AzCopy como AzCopy v8.1.|
| **Directivas de administración del ciclo de vida de Azure Blob storage** | Todavía no se admite |
| **Azure Content Delivery Network (CDN)** | Todavía no se admite|
| **Azure Event Grid** | Todavía no se admite |
| **Búsqueda de Azure** |Todavía no se admite|
| **Explorador de Azure Storage** | Compatibilidad con versiones específicas <br><br>Use solo la versión `1.6.0` o superior. <br>Versión `1.6.0` está disponible como un [descarga gratuita](https://azure.microsoft.com/features/storage-explorer/).|
| **Las ACL del contenedor de BLOB** |Todavía no se admite|
| **Blobfuse** |Todavía no se admite|
| **Dominios personalizados** |Todavía no se admite|
| **Registros de diagnóstico** |Todavía no se admite|
| **Explorador de sistema de archivos** | Compatibilidad limitada |
| **Almacenamiento inmutable** |Todavía no se admite <br><br>Inmutable storage ofrece la capacidad de almacenar datos en un [gusano (escribir una vez, leer muchas)](https://docs.microsoft.com/azure/storage/blobs/storage-blob-immutable-storage) estado.|
| **Niveles de nivel de objeto** |Todavía no se admite <br><br>Por ejemplo: Premium, los niveles frecuente, esporádico y archivo.|
| **Compatibilidad con PowerShell y CLI** | Funcionalidad limitada <br><br>Puede crear una cuenta mediante el uso de Powershell o la CLI. No se puede realizar operaciones o establecer listas de control de acceso en los sistemas de archivos, directorios y archivos.|
| **Sitios Web estáticos** |Todavía no se admite <br><br>En concreto, la capacidad de administrar los archivos [los sitios Web estáticos](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website).|
| **Aplicaciones de terceros** | Compatibilidad limitada <br><br>Aplicaciones de terceros que usan las API de REST para trabajar seguirán funcionando si usas con Data Lake Storage Gen2. <br>Si tiene una aplicación que utiliza la API de Blob, esa aplicación probablemente tendrá problemas si usa esa aplicación con Data Lake Storage Gen2. Para obtener más información, consulte el [API están deshabilitadas para las cuentas de almacenamiento de Gen2 de almacenamiento del lago de datos de almacenamiento de blobs](#blob-apis-disabled) sección de este artículo.|
| **Características de control de versiones** |Todavía no se admite <br><br>Esto incluye [instantáneas](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob) y [eliminación temporal](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete).|
|

