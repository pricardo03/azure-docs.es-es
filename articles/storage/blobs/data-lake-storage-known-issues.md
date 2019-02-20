---
title: Problemas conocidos con Azure Data Lake Storage Gen2 | Microsoft Docs
description: Obtenga información sobre las limitaciones y problemas conocidos con Azure Data Lake Storage Gen2
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 02/07/2018
ms.author: normesta
ms.openlocfilehash: ff158b726c57f4aa5b7822dc0273ab42c350522c
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2019
ms.locfileid: "55895540"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Problemas conocidos con Azure Data Lake Storage Gen2

En este artículo se incluyen problemas conocidos y limitaciones temporales con Azure Data Lake Storage Gen2.

## <a name="api-interoperability"></a>Interoperabilidad API

Las API de Blob Storage y las API de Azure Data Lake Gen2 no son interoperables entre sí.

Si tiene herramientas, aplicaciones, servicios o scripts que usan las API de Blob y quiere usarlas para trabajar con todo el contenido que cargue en su cuenta, no habilite espacios de nombres jerárquicos en su cuenta de Blob Storage hasta que las API de Blob sean interoperables con las API de Azure Data Lake Gen 2. Al usar una cuenta de almacenamiento sin un espacio de nombres jerárquico, carecerá de acceso a características específicas de Data Lake Storage Gen2, como listas de control de acceso al directorio y al sistema de archivos.

## <a name="blob-storage-apis"></a>API de Blob Storage

Las API de Blob Storage aún no están disponibles para las cuentas de Azure Data Lake Storage Gen2.

Estas API están deshabilitadas para evitar problemas de acceso a datos accidental que pueden surgir al seguir siendo inoperables las API de Blob Storage con las API de Azure Data Lake Gen2.

Si usó estas API para cargar datos antes de haberlas deshabilitado y tiene un requisito de producción para tener acceso a esos datos, póngase en contacto con Soporte técnico de Microsoft con la siguiente información:

* Id. de suscripción (el GUID, no el nombre)

* Nombres de la cuenta de almacenamiento

* Si le afecta activamente en producción y, si es así, ¿para qué cuentas de almacenamiento?

* Aunque no le afecte activamente en producción, indíquenos si necesita que estos datos se copien en otra cuenta de almacenamiento por alguna razón y, si es así, cuál es el motivo.

En estas circunstancias, podemos restaurar el acceso a la API de Blob durante un período limitado para que pueda copiar estos datos en una cuenta de almacenamiento que no tenga habilitada la característica de espacios de nombres jerárquicos.

Los discos de máquina virtual (VM) no administrados dependen de las API de Blob Storage, por lo que, si quiere habilitar espacios de nombres jerárquicos en una cuenta de almacenamiento, considere la posibilidad de colocar discos de VM en una cuenta de almacenamiento sin la característica espacios de nombres jerárquicos habilitada.

## <a name="azure-storage-explorer"></a>Explorador de Azure Storage

Para ver o administrar las cuentas de Data Lake Storage Gen2 mediante el Explorador de Azure Storage, debe tener al menos la versión `1.6.0` de la herramienta que está disponible como [descarga gratuita](https://azure.microsoft.com/features/storage-explorer/).

Tenga en cuenta que la versión del explorador de Storage insertada en Azure Portal actualmente no admite la visualización o administración de las cuentas de Data Lake Storage Gen2 con la característica de espacios de nombres jerárquicos habilitada.

## <a name="blob-viewing-tool"></a>Herramienta de visualización de blobs

La herramienta de visualización de blobs de Azure Portal solo cuenta con compatibilidad limitada con Azure Data Lake Storage Gen2.

## <a name="third-party-applications"></a>Aplicaciones de terceros

Las aplicaciones de terceros podrían no admitir Azure Data Lake Storage Gen2.

La compatibilidad queda a discreción de cada proveedor de aplicaciones de terceros. Actualmente, las API de Blob Storage y las API de Azure Data Lake Storage Gen2 no se pueden usar para administrar el mismo contenido. A medida que trabajamos para permitir esa interoperabilidad, es posible que muchas herramientas de terceros admitan automáticamente Azure Data Lake Storage Gen2.

## <a name="azcopy-support"></a>Compatibilidad con AzCopy

La versión 8 de AzCopy no admite Azure Data Lake Storage Gen2.

En su lugar, use la versión preliminar más reciente de AzCopy ([AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)), pues admite los puntos de conexión de Azure Data Lake Storage Gen2.

## <a name="azure-event-grid"></a>Azure Event Grid

[Azure Event Grid](https://azure.microsoft.com/services/event-grid/) no recibe eventos de cuentas de Azure Data Lake Gen2 porque dichas cuentas aún no los generan.  

## <a name="soft-delete-and-snapshots"></a>Eliminación temporal e instantáneas

La eliminación temporal y las instantáneas no están disponibles para las cuentas de Azure Data Lake Storage Gen2.

Todas las características de control de versiones, incluidas [instantáneas](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob) y [eliminación temporal](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) no están aún disponibles para las cuentas de Storage que tienen habilitada la característica de espacios de nombres jerárquicos.

## <a name="object-level-storage-tiers"></a>Capas de almacenamiento de nivel de objeto

Las capas de almacenamiento de nivel de objeto (de acceso frecuente, esporádico y de archivo) aún no están disponibles para las cuentas de Azure Data Lake Storage Gen 2, pero están disponibles para las cuentas de Storage que no tienen habilitada la característica de espacios de nombres jerárquicos.

## <a name="azure-blob-storage-lifecycle-management-policies"></a>Directivas de administración del ciclo de vida de Azure Blob Storage

Las directivas de administración del ciclo de vida de Azure Blob Storage aún no están disponibles para las cuentas de Azure Data Lake Storage Gen2.

Estas directivas están disponibles para las cuentas de Storage que no tienen habilitada la característica de espacios de nombres jerárquicos.

## <a name="diagnostic-logs"></a>Registros de diagnóstico

Los registros de diagnóstico no están disponibles para las cuentas de Azure Data Lake Storage Gen2.
