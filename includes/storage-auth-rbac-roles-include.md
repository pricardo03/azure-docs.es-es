---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/21/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: c7fa81f6d23962eedb3dfeafdd397b62a83d130e
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2019
ms.locfileid: "58372687"
---
Azure proporciona los siguientes roles RBAC integrados para tener acceso a datos de almacenamiento:

- [Propietario del almacenamiento de datos Blob](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner-preview): se usa para establecer la propiedad y administrar el control de acceso POSIX en Azure Data Lake Storage Gen2 (versión preliminar). Para más información, consulte [Control de acceso en Azure Data Lake Storage Gen2](../articles/storage/blobs/data-lake-storage-access-control.md).
- [Colaborador de datos de Blob de almacenamiento](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-contributor-preview): se usa para conceder permisos de lectura, escritura y eliminación a los recursos de almacenamiento de blobs.
- [Lector de datos de Blob de almacenamiento](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-reader-preview): se usa para conceder permisos de solo lectura a los recursos de almacenamiento de blobs.
- [Colaborador de datos de almacenamiento cola](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-contributor-preview): se usa para conceder permisos de lectura, escritura y eliminación a las colas de Azure.
- [Lector de datos de almacenamiento cola](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-reader-preview): se usa para conceder permisos de solo lectura a las colas de Azure.
- [Procesador de mensajes de datos de la cola de almacenamiento](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-processor-preview): Use grant peek, recuperar y permisos de eliminación para los mensajes en colas de Azure Storage.
- [Remitente del mensaje de datos de la cola de almacenamiento](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-sender-preview): Usar para conceder agregar permisos a los mensajes en colas de Azure Storage.

Para más información acerca de cómo se definen los roles integrados para Azure Storage, consulte [Descripción de definiciones de roles](../articles/role-based-access-control/role-definitions.md#management-and-data-operations-preview). Para obtener información acerca de cómo crear roles personalizados de RBAC, consulte [crear roles personalizados para el Control de acceso](../articles/role-based-access-control/custom-roles.md). 
