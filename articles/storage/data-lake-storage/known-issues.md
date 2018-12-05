---
title: Problemas conocidos con Azure Data Lake Storage Gen2 | Microsoft Docs
description: Obtenga información sobre las limitaciones y problemas conocidos con Azure Data Lake Storage Gen2
services: storage
author: normesta
manager: twooley
ms.component: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 11/26/2018
ms.author: normesta
ms.openlocfilehash: fa37c23f936173c19f32b76dffab8908176ebd75
ms.sourcegitcommit: eba6841a8b8c3cb78c94afe703d4f83bf0dcab13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "52621743"
---
# <a name="known-issues-with-azure-data-lake-storage-gen2"></a>Problemas conocidos con Azure Data Lake Storage Gen2

En este artículo se incluyen problemas conocidos y limitaciones temporales con Azure Data Lake Storage Gen2.

## <a name="api-interoperability"></a>Interoperabilidad API

Las API de Blob Storage y las API de Azure Data Lake Gen2 no son interoperables entre sí.

Si necesita usar la misma herramienta para trabajar con todo el contenido que carga en su cuenta, no habilite espacios de nombres jerárquicos en su cuenta de Blob Storage hasta que estas API se vuelvan interoperables entre sí. Al usar una cuenta de almacenamiento sin el espacio de nombres jerárquico, carecerá de acceso a características específicas de Data Lake Storage Gen2, como listas de control de acceso al directorio y al sistema de archivos.

## <a name="blob-storage-apis"></a>API de Blob Storage

Las API de Blob Storage aún no están disponibles para las cuentas de Azure Data Lake Storage Gen2.

Estas API están deshabilitadas para evitar problemas de acceso a datos accidental que pueden surgir al seguir siendo inoperables las API de Blob Storage con las API de Azure Data Lake Gen2.

Los discos de máquina virtual (VM) no administrados dependen de estas API, por lo que si desea habilitar espacios de nombres jerárquicos en una cuenta de almacenamiento, considere la posibilidad de colocar discos de máquina virtual en una cuenta de almacenamiento sin espacios de nombres jerárquicos habilitados.

## <a name="azure-storage-explorer"></a>Explorador de Azure Storage

Algunas características del Explorador de Storage no funcionan aún con los sistemas de archivos de Azure Data Lake Storage Gen2. Estas limitaciones se aplican tanto a la [versión independiente](https://azure.microsoft.com/features/storage-explorer/) del Explorador de Azure Storage como a la versión que aparece en Azure Portal.

## <a name="blob-viewing-tool"></a>Herramienta de visualización de blobs

La herramienta de visualización de blobs de Azure Portal solo cuenta con compatibilidad limitada con Azure Data Lake Storage Gen2.

## <a name="third-party-applications"></a>Aplicaciones de terceros

Las aplicaciones de terceros podrían no admitir Azure Data Lake Storage Gen2.

La compatibilidad queda a discreción de cada proveedor de aplicaciones de terceros. Actualmente, las API de Blob Storage y las API de Azure Data Lake Storage Gen2 no se pueden usar para administrar el mismo contenido. A medida que trabajamos para permitir esa interoperabilidad, es posible que muchas herramientas de terceros admitan automáticamente Azure Data Lake Storage Gen2.

## <a name="azcopy-support"></a>Compatibilidad con AzCopy

La versión 8 de AzCopy no admite Azure Data Lake Storage Gen2.

En su lugar, use la versión preliminar más reciente de AzCopy ([AzCopy v10](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-v10?toc=%2fazure%2fstorage%2ftables%2ftoc.json)), pues admite los puntos de conexión de Azure Data Lake Storage Gen2.

## <a name="oauth-authentication"></a>Autenticación de OAuth

Aún no se han integrado servicios como Azure Databricks, HDInsight y Azure Data Factory con la autenticación de token de portador de OAuth de Azure Active Directory (Azure AD).

## <a name="access-control-lists-acl"></a>Listas de control de acceso (ACL)

Las listas de control de acceso de nivel de archivo y directorio son difíciles de administrar. No hay ninguna herramienta basada en la interfaz de usuario que pueda usar para obtener y establecer esas listas de control de acceso.

## <a name="azure-event-grid"></a>Azure Event Grid

[Azure Event Grid](https://azure.microsoft.com/services/event-grid/) no recibe eventos de cuentas de Azure Data Lake Gen2 porque dichas cuentas aún no los generan.  

## <a name="role-based-access-control"></a>Control de acceso basado en rol

No puede aplicar control de acceso basado en rol a objetos del sistema de archivos en una cuenta de Azure Data Lake Storage Gen2.

## <a name="sql-data-warehouse-polybase"></a>PolyBase de SQL Data Warehouse

Cuando los firewalls de Storage están habilitados en una cuenta de Azure Storage, [PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide?view=sql-server-2017) de SQL Data Warehouse no puede tener acceso a esas cuentas.

## <a name="soft-delete-and-snapshots"></a>Eliminación temporal e instantáneas

La eliminación temporal y las instantáneas no están disponibles para las cuentas de Azure Data Lake Storage Gen2.

Todas las características de control de versiones, incluidas [instantáneas](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob) y [eliminación temporal](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) no están aún disponibles para las cuentas de Storage con espacios de nombres jerárquicos habilitados.

## <a name="object-level-storage-tiers"></a>Capas de almacenamiento de nivel de objeto

Las capas de almacenamiento de nivel de objeto (de acceso frecuente, esporádico y de archivo) aún no están disponibles para las cuentas de Azure Data Lake Storage Gen2, pero están disponibles para las cuentas de Storage sin espacios jerárquicos habilitados.

## <a name="azure-blob-storage-lifecycle-management-preview-policies"></a>Directivas de administración del ciclo de vida de Azure Blob Storage (versión preliminar)

Las directivas de administración del ciclo de vida de Azure Blob Storage aún no están disponibles para las cuentas de Azure Data Lake Storage Gen2.

Estas directivas están disponibles para las cuentas de Storage sin espacios jerárquicos habilitados.

## <a name="diagnostic-logs"></a>Registros de diagnóstico

Los registros de diagnóstico no están disponibles para las cuentas de Azure Data Lake Storage Gen2.

Para solicitar registros de diagnóstico, póngase en contacto con el servicio de soporte técnico de Azure. Proporcióneles su nombre de cuenta y el período de tiempo para el que requiere los registros.
