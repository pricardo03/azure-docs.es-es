---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 02/19/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 9402c4b24c9d64b4b69d750fbd19de40cda396f3
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57218097"
---
Azure proporciona los siguientes roles RBAC integrados para tener acceso a datos de almacenamiento:

- [Propietario de datos de blobs de almacenamiento (versión preliminar)](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner-preview): se usa para establecer la propiedad y administrar el control de acceso POSIX en Azure Data Lake Storage Gen2 (versión preliminar). Para más información, consulte [Control de acceso en Azure Data Lake Storage Gen2](../articles/storage/blobs/data-lake-storage-access-control.md).
- [Colaborador de datos de blobs de almacenamiento (versión preliminar)](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-contributor-preview): se usa para conceder permisos de lectura, escritura y eliminación a los recursos de almacenamiento de blobs.
- [Lector de datos de blobs de almacenamiento (versión preliminar)](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-reader-preview): se usa para conceder permisos de solo lectura a los recursos de almacenamiento de blobs.
- [Colaborador de datos de la cola de almacenamiento (versión preliminar)](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-contributor-preview): se usa para conceder permisos de lectura, escritura y eliminación a las colas de Azure.
- [Lector de datos de la cola de almacenamiento (versión preliminar)](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-reader-preview): se usa para conceder permisos de solo lectura a las colas de Azure.

Para más información acerca de cómo se definen los roles integrados para Azure Storage, consulte [Descripción de definiciones de roles](../articles/role-based-access-control/role-definitions.md#management-and-data-operations-preview). Para obtener información acerca de cómo crear roles personalizados de RBAC, consulte [crear roles personalizados para el Control de acceso](../articles/role-based-access-control/custom-roles.md). 
