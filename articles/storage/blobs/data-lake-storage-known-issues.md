---
title: Problemas conocidos con Azure Data Lake Storage Gen2 | Microsoft Docs
description: Obtenga información sobre las limitaciones y problemas conocidos con Azure Data Lake Storage Gen2
services: storage
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: normesta
ms.openlocfilehash: 5677649b8f002490900ec32bee954348b2f444e6
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "55731554"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Problemas conocidos con Azure Data Lake Storage Gen2

En este artículo se incluyen problemas conocidos y limitaciones temporales con Azure Data Lake Storage Gen2.

## <a name="api-interoperability"></a>Interoperabilidad API

Las API de Blob Storage y las API de Azure Data Lake Gen2 no son interoperables entre sí.

Si tiene herramientas personalizadas, aplicaciones o scripts que usan las API de Blob y desea utilizarlas para trabajar con todo el contenido que cargue en su cuenta, no habilite espacios de nombres jerárquicos en su cuenta de Blob Storage hasta que las API de Blob sean interoperables con las API de Azure Data Lake Gen 2. Al usar una cuenta de almacenamiento sin el espacio de nombres jerárquico, carecerá de acceso a características específicas de Data Lake Storage Gen2, como listas de control de acceso al directorio y al sistema de archivos.

## <a name="blob-storage-apis"></a>API de Blob Storage

Las API de Blob Storage aún no están disponibles para las cuentas de Azure Data Lake Storage Gen2.

Estas API están deshabilitadas para evitar problemas de acceso a datos accidental que pueden surgir al seguir siendo inoperables las API de Blob Storage con las API de Azure Data Lake Gen2.

Si usó estas API para cargar datos antes de haberlas deshabilitado y tiene un requisito de producción para tener acceso a esos datos, póngase en contacto con Soporte técnico de Microsoft con la siguiente información:

* Id. de suscripción (el GUID, no el nombre)

* Nombres de la cuenta de almacenamiento

* Si le afecta activamente en producción y, si es así, ¿para qué cuentas de almacenamiento?

* Aunque no le afecte activamente en producción, indíquenos si necesita que estos datos se copien en otra cuenta de almacenamiento por alguna razón y, si es así, cuál es el motivo.

En estas circunstancias, podemos restaurar el acceso a la API de Blob durante un período de tiempo limitado para que pueda copiar estos datos en una cuenta de almacenamiento que no tenga espacios de nombres jerárquicos habilitados.

Los discos de máquina virtual (VM) no administrados dependen de las API de Blob Storage, por lo que, si quiere habilitar espacios de nombres jerárquicos en una cuenta de almacenamiento, considere la posibilidad de colocar discos de VM en una cuenta de almacenamiento sin espacios de nombres jerárquicos habilitados.

## <a name="azure-storage-explorer"></a>Explorador de Azure Storage

Para ver o administrar las cuentas de Data Lake Storage Gen2 mediante el Explorador de Azure Storage, debe tener al menos la versión `1.6.0` de la herramienta que está disponible como [descarga gratuita](https://azure.microsoft.com/features/storage-explorer/).

Tenga en cuenta que la versión del explorador de Storage insertada en Azure Portal actualmente no admite la visualización o administración de las cuentas de Data Lake Storage Gen2 con espacios de nombres jerárquicos habilitados.

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

Todas las características de control de versiones, incluidas [instantáneas](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob) y [eliminación temporal](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) no están aún disponibles para las cuentas de Storage con espacios de nombres jerárquicos habilitados.

## <a name="object-level-storage-tiers"></a>Capas de almacenamiento de nivel de objeto

Las capas de almacenamiento de nivel de objeto (de acceso frecuente, esporádico y de archivo) aún no están disponibles para las cuentas de Azure Data Lake Storage Gen 2, pero están disponibles para las cuentas de Storage sin espacios de nombres jerárquicos habilitados.

## <a name="azure-blob-storage-lifecycle-management-preview-policies"></a>Directivas de administración del ciclo de vida de Azure Blob Storage (versión preliminar)

Las directivas de administración del ciclo de vida de Azure Blob Storage aún no están disponibles para las cuentas de Azure Data Lake Storage Gen2.

Estas directivas están disponibles para las cuentas de Storage sin espacios de nombres jerárquicos habilitados.

## <a name="diagnostic-logs"></a>Registros de diagnóstico

Los registros de diagnóstico no están disponibles para las cuentas de Azure Data Lake Storage Gen2.
