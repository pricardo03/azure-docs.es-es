---
title: Problemas conocidos con Azure Data Lake Storage Gen2 | Microsoft Docs
description: Obtenga información sobre las limitaciones y problemas conocidos con Azure Data Lake Storage Gen2
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 02/28/2019
ms.author: normesta
ms.openlocfilehash: 51230fe050c67253dd5b2bb3f23d03512df64ef6
ms.sourcegitcommit: c53a800d6c2e5baad800c1247dce94bdbf2ad324
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64939248"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Problemas conocidos con Azure Data Lake Storage Gen2

En este artículo contiene problemas conocidos y limitaciones temporales con Data Lake Storage Gen2.

## <a name="sdk-support-for-data-lake-storage-gen2-accounts"></a>Compatibilidad con el SDK para las cuentas de Data Lake Storage Gen2

No existen SDK que funcionan con cuentas de Data Lake Storage Gen2.

## <a name="blob-storage-apis"></a>API de Blob Storage

API de BLOB storage aún no están disponibles para las cuentas de Data Lake Storage Gen2.

Estas API están deshabilitadas para evitar problemas de acceso a datos accidental que pueden surgir al seguir siendo inoperables las API de Blob Storage con las API de Azure Data Lake Gen2.

Si usó estas API para cargar datos antes de haberlas deshabilitado y tiene un requisito de producción para tener acceso a esos datos, póngase en contacto con Soporte técnico de Microsoft con la siguiente información:

* Id. de suscripción (el GUID, no el nombre)

* Nombres de la cuenta de almacenamiento

* Si le afecta activamente en producción y, si es así, ¿para qué cuentas de almacenamiento?

* Aunque no le afecte activamente en producción, indíquenos si necesita que estos datos se copien en otra cuenta de almacenamiento por alguna razón y, si es así, cuál es el motivo.

En estas circunstancias, podemos restaurar el acceso a la API de Blob durante un período limitado para que pueda copiar estos datos en una cuenta de almacenamiento que no tenga habilitada la característica de espacios de nombres jerárquicos.

Los discos de máquina virtual (VM) no administrados dependen de las API de Blob Storage, por lo que, si quiere habilitar espacios de nombres jerárquicos en una cuenta de almacenamiento, considere la posibilidad de colocar discos de VM en una cuenta de almacenamiento sin la característica espacios de nombres jerárquicos habilitada.

## <a name="api-interoperability"></a>Interoperabilidad API

Las API de Blob Storage y las API de Azure Data Lake Gen2 no son interoperables entre sí.

Si tiene herramientas, aplicaciones, servicios o scripts que usan las API de Blob y quiere usarlas para trabajar con todo el contenido que cargue en su cuenta, no habilite espacios de nombres jerárquicos en su cuenta de Blob Storage hasta que las API de Blob sean interoperables con las API de Azure Data Lake Gen 2. Con una cuenta de almacenamiento sin un espacio de nombres jerárquico significa que, a continuación, no tiene acceso a características específicas de Data Lake Storage Gen2, como archivos y directorios listas de control de acceso del sistema.

## <a name="azure-storage-explorer"></a>Explorador de Azure Storage

Para ver o administrar las cuentas de Data Lake Storage Gen2 mediante el Explorador de Azure Storage, debe tener al menos la versión `1.6.0` de la herramienta que está disponible como [descarga gratuita](https://azure.microsoft.com/features/storage-explorer/).

Tenga en cuenta que la versión del explorador de Storage que se incrusta en el portal de Azure actualmente no se admite ver o administrar las cuentas de Data Lake Storage Gen2 con la característica de espacio de nombres jerárquico habilitada.

## <a name="blob-viewing-tool"></a>Herramienta de visualización de blobs

Herramienta de visualización en el portal de Azure de BLOB solo tiene compatibilidad para Data Lake Storage Gen2 limitada.

## <a name="third-party-applications"></a>Aplicaciones de terceros

Las aplicaciones de terceros podrían no admitir Data Lake Storage Gen2.

La compatibilidad queda a discreción de cada proveedor de aplicaciones de terceros. Actualmente, las API de almacenamiento de blobs y API de Data Lake Storage Gen2 no se puede usar para administrar el mismo contenido. Mientras trabajamos para permitir la interoperabilidad de ese, es posible que muchas herramientas de terceros admitirá automáticamente Data Lake Storage Gen2.

## <a name="azcopy-support"></a>Compatibilidad con AzCopy

AzCopy versión 8 no es compatible con Data Lake Storage Gen2.

En su lugar, use la versión preliminar más reciente de AzCopy ( [AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json) ) que sea compatible con los puntos de conexión de Data Lake Storage Gen2.

## <a name="azure-event-grid"></a>Azure Event Grid

[Azure Event Grid](https://azure.microsoft.com/services/event-grid/) no recibe eventos de cuentas de Azure Data Lake Gen2 porque dichas cuentas aún no los generan.  

## <a name="soft-delete-and-snapshots"></a>Eliminación temporal e instantáneas

Las instantáneas y la eliminación temporal no están disponibles para las cuentas de Data Lake Storage Gen2.

Todas las características de control de versiones, incluidas [instantáneas](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob) y [eliminación temporal](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) no están aún disponibles para las cuentas de Storage que tienen habilitada la característica de espacios de nombres jerárquicos.

## <a name="object-level-storage-tiers"></a>Capas de almacenamiento de nivel de objeto

Las capas de almacenamiento de nivel de objeto (de acceso frecuente, esporádico y de archivo) aún no están disponibles para las cuentas de Azure Data Lake Storage Gen 2, pero están disponibles para las cuentas de Storage que no tienen habilitada la característica de espacios de nombres jerárquicos.

## <a name="azure-blob-storage-lifecycle-management-policies"></a>Directivas de administración del ciclo de vida de Azure Blob Storage

Directivas de administración de ciclo de vida de Azure Blob Storage aún no están disponibles para las cuentas de Data Lake Storage Gen2.

Estas directivas están disponibles para las cuentas de Storage que no tienen habilitada la característica de espacios de nombres jerárquicos.

## <a name="diagnostic-logs"></a>Registros de diagnóstico

Los registros de diagnóstico no están disponibles para las cuentas de Data Lake Storage Gen2.
